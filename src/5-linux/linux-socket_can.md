# SocketCAN
The socketcan package is an implementation of CAN protocols (Controller Area Network) for Linux. CAN is a networking technology which has widespread use in automation, embedded devices, and automotive fields.SocketCAN uses the Berkeley socket API, the Linux network stack and implements the CAN device drivers as network interfaces. The CAN socket API has been designed as similar as possible to the TCP/IP protocols to allow programmers, familiar with network programming, to easily learn how to use CAN sockets.

Main goal of SocketCAN is to provide a socket interface to user space applications which builds upon the Linux network layer. In contrast to the commonly known TCP/IP and ethernet networking, the CAN bus is a broadcast-only medium that has no MAC-layer addressing like ethernet. The CAN-identifier (can_id) is used for arbitration on the CAN-bus. Therefore the CAN-IDs have to be chosen uniquely on the bus. When designing a CAN-ECU network the CAN-IDs are mapped to be sent by a specific ECU. For this reason a CAN-ID can be treated best as a kind of source address.


This chapter is an extraction of the kernel documentation and will explain basic implementation of SocketCan, to see complete documentation please enter here:  [Kernel Docs](https://docs.kernel.org/networking/can.html#socketcan-concept)


## How to use SocketCAN

Like TCP/IP, you first need to open a socket for communicating over a CAN network and pass PF_CAN as the first argument to the socket system call. Currently, there are two CAN protocols to choose from, the raw socket protocol and the broadcast manager (BCM). So to open a socket, you would write:

```
s = socket(PF_CAN, SOCK_RAW, CAN_RAW);
```
and:
```
s = socket(PF_CAN, SOCK_DGRAM, CAN_BCM);
```
respectively. After the successful creation of the socket, you would normally use the bind system call to bind the socket to a CAN interface. After binding (CAN_RAW) or connecting (CAN_BCM) the socket, you can read and write from/to the socket or use send, sendto, sendmsg and the recv* counterpart operations on the socket as usual. There are also CAN specific socket options described below.

The Classical CAN frame structure (aka CAN 2.0B), the CAN FD frame structure and the sockaddr structure are defined in include/linux/can.h:
```
struct can_frame {
        canid_t can_id;  /* 32 bit CAN_ID + EFF/RTR/ERR flags */
        union {
                /* CAN frame payload length in byte (0 .. CAN_MAX_DLEN)
                 * was previously named can_dlc so we need to carry that
                 * name for legacy support
                 */
                __u8 len;
                __u8 can_dlc; /* deprecated */
        };
        __u8    __pad;   /* padding */
        __u8    __res0;  /* reserved / padding */
        __u8    len8_dlc; /* optional DLC for 8 byte payload length (9 .. 15) */
        __u8    data[8] __attribute__((aligned(8)));
};
```
Remark: The len element contains the payload length in bytes and should be used instead of can_dlc. The deprecated can_dlc was misleadingly named as it always contained the plain payload length in bytes and not the so called 'data length code' (DLC).

To pass the raw DLC from/to a Classical CAN network device the len8_dlc element can contain values 9 .. 15 when the len element is 8 (the real payload length for all DLC values greater or equal to 8).

The alignment of the (linear) payload data[] to a 64bit boundary allows the user to define their own structs and unions to easily access the CAN payload. There is no given byteorder on the CAN bus by default. A read(2) system call on a CAN_RAW socket transfers a struct can_frame to the user space.

The sockaddr_can structure has an interface index like the PF_PACKET socket, that also binds to a specific interface:
```
struct sockaddr_can {
        sa_family_t can_family;
        int         can_ifindex;
        union {
                /* transport protocol class address info (e.g. ISOTP) */
                struct { canid_t rx_id, tx_id; } tp;

                /* J1939 address information */
                struct {
                        /* 8 byte name when using dynamic addressing */
                        __u64 name;

                        /* pgn:
                         * 8 bit: PS in PDU2 case, else 0
                         * 8 bit: PF
                         * 1 bit: DP
                         * 1 bit: reserved
                         */
                        __u32 pgn;

                        /* 1 byte address */
                        __u8 addr;
                } j1939;

                /* reserved for future CAN protocols address information */
        } can_addr;
};
```
To determine the interface index an appropriate ioctl() has to be used (example for CAN_RAW sockets without error checking):
```
int s;
struct sockaddr_can addr;
struct ifreq ifr;

s = socket(PF_CAN, SOCK_RAW, CAN_RAW);

strcpy(ifr.ifr_name, "can0" );
ioctl(s, SIOCGIFINDEX, &ifr);

addr.can_family = AF_CAN;
addr.can_ifindex = ifr.ifr_ifindex;

bind(s, (struct sockaddr *)&addr, sizeof(addr));

(..)
```
To bind a socket to all CAN interfaces the interface index must be 0 (zero). In this case the socket receives CAN frames from every enabled CAN interface. To determine the originating CAN interface the system call recvfrom may be used instead of read(2). To send on a socket that is bound to 'any' interface sendto is needed to specify the outgoing interface.

Reading CAN frames from a bound CAN_RAW socket (see above) consists of reading a struct can_frame:

```
struct can_frame frame;

nbytes = read(s, &frame, sizeof(struct can_frame));

if (nbytes < 0) {
        perror("can raw socket read");
        return 1;
}

/* paranoid check ... */
if (nbytes < sizeof(struct can_frame)) {
        fprintf(stderr, "read: incomplete CAN frame\n");
        return 1;
}

/* do something with the received CAN frame */
```
Writing CAN frames can be done similarly, with the write(2) system call:
```
nbytes = write(s, &frame, sizeof(struct can_frame));
When the CAN interface is bound to 'any' existing CAN interface (addr.can_ifindex = 0) it is recommended to use recvfrom(2) if the information about the originating CAN interface is needed:

struct sockaddr_can addr;
struct ifreq ifr;
socklen_t len = sizeof(addr);
struct can_frame frame;

nbytes = recvfrom(s, &frame, sizeof(struct can_frame),
                  0, (struct sockaddr*)&addr, &len);

/* get interface name of the received CAN frame */
ifr.ifr_ifindex = addr.can_ifindex;
ioctl(s, SIOCGIFNAME, &ifr);
printf("Received a CAN frame from interface %s", ifr.ifr_name);
To write CAN frames on sockets bound to 'any' CAN interface the outgoing interface has to be defined certainly:

strcpy(ifr.ifr_name, "can0");
ioctl(s, SIOCGIFINDEX, &ifr);
addr.can_ifindex = ifr.ifr_ifindex;
addr.can_family  = AF_CAN;

nbytes = sendto(s, &frame, sizeof(struct can_frame),
                0, (struct sockaddr*)&addr, sizeof(addr));
An accurate timestamp can be obtained with an ioctl(2) call after reading a message from the socket:

```
struct timeval tv;
ioctl(s, SIOCGSTAMP, &tv);
```
The timestamp has a resolution of one microsecond and is set automatically at the reception of a CAN frame.

