# ACTIVITY TIME: Simple calculator

With the commands mentioned above we can start working on our first versioned project.
Now assume you found an interesting code for a simple calculator(calculator.c).
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

## Incremental change 1
It only has the addition operation but we want to multiply as well. So, to record our progress, lets implement git.
Note: Remember to compile it as >gcc calculator.c -o executable

Hints:
Create a new git repository.
Check/update your user/email configuration(only on first time)
Add calculator.c

## Incremental change 2
Style changes:

- goto should not be used, modify it
- readability

# Incremental change 3
Modular calculator:

- Divide calculator in different files for maintainability