# ACTIVITY TIME: Simple calculator

With the commands mentioned in the past pages we can start working on our first versioned project.
Now assume you found an interesting code for a simple calculator(calculator.c).
Note: Remember to compile it as $gcc calculator.c -o executableName
```
    #include <stdio.h>

    void add ();
    void sel_func (int);

    int main (void)
    {
            int s;
        
            Input:
            printf("Select the number of calculator operation [ 1-sum ] : ");
            scanf("%d",&s);
            
            if (s > 1 | s < 1){
                    printf("Please select a valid operation\n");
                    goto Input;
            }
            sel_func (s);
        
            goto Input;
    }

    void sel_func (int s)
    {
            void (*fptr)(void);
            switch (s){
            case 1:
                    fptr = add;
                    break;    
            }
        
            fptr();
    }

    void add ()
    {
            int a, b;
            
            printf("Input two numbers : \n");
            scanf("%d%d", &a, &b);
            printf("Result = %d\n", a + b);
    }
```
Lets create a new repository and add this as the first commit.
Hints:
Check/update your user/email configuration(only on first time)
Add calculator.c

## Incremental change 1
Our first calculator only has the addition operation but we want to multiply as well.

## Incremental change 2
Style changes:

- goto should not be used, modify it
- readability

# Incremental change 3
Modular calculator:
- Divide calculator in different files for maintainability. 
- Make any other improvement that adds value to a good design pattern.