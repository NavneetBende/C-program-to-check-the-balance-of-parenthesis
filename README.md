Balanced Parenthesis in C
To check balanced parenthesis is a basic interview question where we are asked to find whether the given string(of brackets) is balanced or not. To do this, the traditional way of doing is using stacks (implemented using array). Different brackets are  ( ) , [ ] , { }. Question can be asked on any type of bracket or of all types of brackets. It is also called as missing braces program in C

C program to check balanced parenthesis
Program to Implement Balancing of Parenthesis using Stack in C
We will discuss two methods one with a global stack and another with dynamic memory created stack.

Algorithm :
Declare a structure for character stack.
Now traverse the expression string exp.
If the current character is a starting bracket (‘(‘ or ‘{‘ or ‘[‘) then push it to stack.
If the current character is a closing bracket (‘)’ or ‘}’ or ‘]’) then pop from stack and if the popped character is the matching starting bracket then fine else brackets are not balanced.
After complete traversal, if there is some starting bracket left in stack then “NOT BALANCED”
Balanced-Parenthesis-problem-C
Method 1
Run
#include <stdio.h>
#include <stdlib.h>

#define MAX 100

struct stack {
  char stck[MAX];
  int top;
}s;

void push(char item) {
  if (s.top == (MAX - 1))
    printf("Stack is Full\n");

  else {
    s.top = s.top + 1;
    s.stck[s.top] = item;
  }
}

void pop() {
  if (s.top == -1)
    printf("Stack is Empty\n");

  else
    s.top = s.top - 1;
}

int checkPair(char val1,char val2){
    return (( val1=='(' && val2==')' )||( val1=='[' && val2==']' )||( val1=='{' && val2=='}' ));
}

int checkBalanced(char expr[], int len){
      
    for (int i = 0; i < len; i++)  
    { 
        if (expr[i] == '(' || expr[i] == '[' || expr[i] == '{')  
        {  
          push(expr[i]); 
        } 
        else
        {
            // exp = {{}}}
            // if you look closely above {{}} will be matched with pair, Thus, stack "Empty"
            //but an extra closing parenthesis like '}' will never be matched
            //so there is no point looking forward
        if (s.top == -1) 
            return 0;
        else if(checkPair(s.stck[s.top],expr[i]))
        {
            pop();
            continue;
        }
        // will only come here if stack is not empty
        // pair wasn't found and it's some closing parenthesis
        //Example : {{}}(]
        return 0;
        }
    }    
    return 1; 
}
int main() {
  char exp[MAX] = "({})[]{}";
  int i = 0;
  s.top = -1;

  int len = strlen(exp);
  checkBalanced(exp, len)?printf("Balanced"): printf("Not Balanced"); 

  return 0;
}
Output :
INPUT THE STRING : ()(){{}}

BALANCED EXPRESSION
Method 2
We will look at another method where we will create a stack with dynamic memory allocation and not global elements.
Run

#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>
#include <math.h>
#define MAX 100

// A structure to represent a stack 
struct Stack { 
    int top; 
    int maxSize; 
    char* array; 
}; 

struct Stack* create(char max) 
{ 
    struct Stack* stack = (struct Stack*)malloc(sizeof(struct Stack)); 
    stack->maxSize = max; 
    stack->top = -1; 
    stack->array = (char*)malloc(stack->maxSize * sizeof(char));
    // here above memory for array is being created
    // size would be 10*4 = 40
    return stack; 
}

// Checking with this function is stack is full or not
// Will return true is stack is full else false 
//Stack is full when top is equal to the last index 
int isFull(struct Stack* stack) 
{ 
    if(stack->top == stack->maxSize - 1){
        printf("Will not be able to push maxSize reached\n");
    }
    // Since array starts from 0, and maxSize starts from 1
    return stack->top == stack->maxSize - 1; 
}

// By definition the Stack is empty when top is equal to -1 
// Will return true if top is -1
int isEmpty(struct Stack* stack) 
{ 
    return stack->top == -1; 
}

// Push function here, inserts value in stack and increments stack top by 1
void push(struct Stack* stack, char item) 
{ 
    if (isFull(stack)) 
        return; 
    stack->array[++stack->top] = item; 
}

// Function to remove an item from stack.  It decreases top by 1 
void pop(struct Stack* stack) 
{ 
    if (isEmpty(stack)) 
        return; 
    
    stack->array[stack->top--]; 
} 

// Function to return the top from stack without removing it 
int peek(struct Stack* stack) 
{ 
    if (isEmpty(stack)) 
        return INT_MIN; 
    return stack->array[stack->top]; 
} 

bool checkPair(char val1,char val2){
    return (( val1=='(' && val2==')' )||( val1=='[' && val2==']' )||( val1=='{' && val2=='}' ));
}

bool checkBalanced(struct Stack* stack, char expr[], int len){
      
    for (int i = 0; i < len; i++)  
    { 
        if (expr[i] == '(' || expr[i] == '[' || expr[i] == '{')  
        {  
          push(stack, expr[i]); 
        } 
        else
        {
            // exp = {{}}}
            // if you look closely above {{}} will be matched with pair, Thus, stack "Empty"
            //but an extra closing parenthesis like '}' will never be matched
            //so there is no point looking forward
        if (isEmpty(stack)) 
            return false;
        else if(checkPair(peek(stack),expr[i]))
        {
            pop(stack);
            continue;
        }
        // will only come here if stack is not empty
        // pair wasn't found and it's some closing parenthesis
        //Example : {{}}(]
        return false;
        }
    }    
    return true; 
}
int main() {
  char exp[MAX] = "({})[]";

  int len = strlen(exp);
  struct Stack* stack = create(len); 
  checkBalanced(stack, exp, len)?printf("Balanced"): printf("Not Balanced"); 

  return 0;
}
Output
Balanced
