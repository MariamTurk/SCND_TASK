# SCND_TASK

///NAME : Mariam Turk
/// ID: 1211115
/// sec :1
/// Instructor : Ahmad Abusnaina

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <math.h>
#include <ctype.h>
#include <stdbool.h>
#define MAX 150

typedef struct TreeNode
{
    char* data;  // Operator or operand
    struct TreeNode* left;
    struct TreeNode* right;
} TreeNode;

typedef struct NODE
{
    char string[MAX];
    struct NODE * NEXT;
    struct NODE * PREV;
};



typedef struct StackNode {
    TreeNode* tree;
    struct StackNode* next;
} StackNode;


//---------------------------------------------------------------------------------------------------------------------------------------------------------------------


///all functions
void pushT(char data);
int popT();
void pushLetter(char data);
char popLetter();
void printStackLetter();
int priority(char op);
int isdigitt(char c);
int isOperation(char c);
int OpLetter(char op);
void makeStackEmpty();
int isEmptyLetter();
bool isOperator(char ch);
StackNode* createStackNode(TreeNode* tree);
void push(StackNode** top, TreeNode* tree);
TreeNode* pop(StackNode** top);
TreeNode* createTnode(char data );
void freeTree(TreeNode* root);
struct NODE* createNode(char string[31]);
struct NODE * createlist();
void Insert_At_End( char * str,struct NODE * node);
void clearWS_SC(char* string);
char* preConverter(char* VALID);
TreeNode* stack_ET(char* postfixExp);
int calu_tree(TreeNode*tree);
int evaluatePostfix(char* postfixExp);
void loadFromFile(struct NODE* head, const char* filename);
void print_menu ();




///functions for stack
//-----------------------------------------------------------------------------------------------------------------------------------------------------------------
int top = -1;
char stack[MAX]; // this stack for make the postfix of expressions
int stacT[MAX]; // this is the stack used for tree
int topT = -1;

void pushT(char data)
{
    topT++;
    stacT[topT] = data;
}

int popT()
{
    int value = stacT[topT];
    topT--;
    return value;
}

// to insert an elemnte to the stack
void pushLetter(char data)
{
    top++;
    stack[top] = data;
}

// to delete an elemnte to the stack
char popLetter()
{
    char value = stack[top];
    top--;
    return value;
}

//print the stack
void printStackLetter()
{
    int i;
    if (top == -1)
        return;
    for (i = 0; i <= top; i++)
        printf("%c ", stack[i]);
    printf("\n");
}




// checks the priority of the operations
int priority(char op)
{
    if (op == '+' || op == '-')
        return 1;
    else if (op == '*' || op == '/' )
        return 2;
    else if (op == '%' )
        return 2;

    return 0;
}


// checks if this char is a digit or not
int isdigitt(char c)
{
    return (c >= '0' && c <= '9');
}


// checks if this char is an operation or not
int isOperation(char c)
{
    if (c =='*' || c=='+' || c=='%' || c=='-' || c=='/'  ) return 1;
    else return 0;
}

// the function returns an integer value: 1 if the character is an operand, and 0 if it is not.
int OpLetter(char op)
{
    if ((op >= '0' && op <= '9') || (op >= 'A' && op <= 'Z') || (op >= 'a' && op <= 'z'))
        return 1;
    else
        return 0;
}
// makes the stack empty
void makeStackEmpty()
{
    // Reset the top index to -1
    top = -1;
}


// checks if the stack empty or not
int isEmptyLetter()
{
    if (top == -1)
        return 1;
    else
        return 0;
}

bool isOperator(char ch) {
    return (ch == '+' || ch == '-' || ch == '*' || ch == '/');
}

// create this function to make tree by using struct stack
StackNode* createStackNode(TreeNode* tree) {
    StackNode* node = (StackNode*)malloc(sizeof(StackNode));
    node->tree = tree;
    node->next = NULL;
    return node;
}


void push(StackNode** top, TreeNode* tree) {
    StackNode* node = createStackNode(tree);
    node->next = *top;
    *top = node;
}


TreeNode* pop(StackNode** top) {
    if (*top == NULL)
        return NULL;

    StackNode* temp = *top;
    TreeNode* tree = temp->tree;
    *top = (*top)->next;
    free(temp);
    return tree;
}


//-----------------------------------------------------------------------------------------------------------------------------------------------------------------



///functions for tree
//-----------------------------------------------------------------------------------------------------------------------------------------------------------------
TreeNode* createTnode(char data )
{
    TreeNode* newNode = (TreeNode*)malloc(sizeof(TreeNode)); //<-- will search about a space in the memo
    if (newNode == NULL)
    {
        printf("memory allocation failed");
        return NULL;
    }
    newNode->data = data;     // set the data inside the node.
    //--------->> note that at the previous line, it showed the importance of adding it at a struct instead of writing all of them
    newNode->left = NULL;    //set the lift pointer to null temprorey.
    newNode->right = NULL;  //set the right pointer to null temprorey.
    return(newNode);
}

void freeTree(TreeNode* root) {
    if (root == NULL) {
        return;
    }

    // Free the left and right subtrees
    freeTree(root->left);
    freeTree(root->right);

    // Free the current node
    free(root);
}


//-----------------------------------------------------------------------------------------------------------------------------------------------------------------



///functions of linked list
//---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
// function to create new node
struct NODE* createNode(char string[31])
{
    struct NODE* newNode = (struct NODE*)malloc(sizeof(struct NODE));
    //put the string in the node
    strcpy(newNode->string, string);
    newNode->NEXT = NULL;
    return newNode;
}


//to create a new double linked list*/
 struct NODE * createlist()
 {
   struct NODE * l= (struct NODE *)malloc(sizeof(struct NODE));
   //in case the list is empty
   if(l==NULL)
   {
       printf("\n Out of memory \n");
       return NULL;
   }
    l->NEXT=NULL;
    l->PREV=NULL;
    return l;
  }


// function to insert at end of linked list
void Insert_At_End( char * str,struct NODE * node)
{

    struct NODE *NEWNODE = (struct NODE*)malloc(sizeof(struct NODE));
    strcpy(NEWNODE-> string, str);
    NEWNODE -> NEXT = NULL;
    //in case the list is empty
    if(node == NULL)
    {
        NEWNODE -> PREV = NULL;
    }
    // start to insert from the end of the list (in case the list is not empty)
    struct NODE * LASTNODE = node;
    while(LASTNODE -> NEXT != NULL)
    {
        LASTNODE = LASTNODE -> NEXT;
    }
    LASTNODE -> NEXT = NEWNODE;
    NEWNODE -> PREV = LASTNODE;
}

//-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



///functions of the project
//-----------------------------------------------------------------------------------------------------------------------------------------------------------------


//function to clear white space and special	characters
void clearWS_SC(char* string)
{
    int i, j;
    for (i = 0, j = 0; string[i] != '\0'; i++)
    {
        // Skip white spaces and some special characters
        if (!isspace(string[i]) && isalnum(string[i]) || string[i] == '+' || string[i] == '-' || string[i] == '%' || string[i] == '*' || string[i] == '/' ||string[i] == '('||string[i] == ')')
        {
            string[j] = string[i];
            j++;
        }
    }
    string[j] = '\0'; // Add null terminator to the end of the modified string
}



// function to convert the infix to postfix
char* preConverter(char* VALID)
{
    makeStackEmpty();
    // to clear the white spaces and Special Characters
    clearWS_SC(VALID);
    int i, j;
    // to save the postfix exp in it
    char* PRE = (char*)malloc(MAX * sizeof(char));
    if (PRE == NULL) {
        printf("Memory allocation failed.\n");
        exit(EXIT_FAILURE);
    }
    // this is the char which will be sorting the data inside the new pre
    char temp;

    // to put the exp in an array temp
    for (i = 0, j = 0; VALID[i] != '\0'; i++)
    {
        temp = VALID[i];

        // if the temp is a letter or number push it in the stack
        if (OpLetter(temp))
        {
            PRE[j++] = temp;
        }

        // if the temp is an operation (+ - / * % ) check the priority if the priority of the Operation of the stack is greater than the priority of the Operation in temp pop it, else push it to the stack
        else if (temp == '+' || temp == '-' || temp == '*' || temp == '/' || temp == '%')
        {
            while (top >= 0 && priority(stack[top]) > priority(temp))
            {
                // the priority of the Operation of the stack is greater
                PRE[j++] = popLetter();
            }
            // the priority of the Operation of the stack is not greater
            PRE[j++] = ' ';
            pushLetter(temp);
            PRE[j++] = ' ';
        }

        // if the temp is '(' just push it on the stack
        else if (temp == '(')
        {
            pushLetter(temp);
        }

        // if temp is ')' pop all things in the stack until the stack reaches the '('
        else if (temp == ')')
        {
            while (top >= 0 && stack[top] != '(')
            {
                PRE[j++] = popLetter();
            }

            // if the top of stack is '(' pop it
            if (top >= 0 && stack[top] == '(')
            {
                popLetter();
            }
        }
    }

    // if the stack is not empty and doesn't have ')' or '(', pop all operations from it
    while (top >= 0)
    {
        PRE[j++] = popLetter();
    }

    PRE[j] = '\0';

    // Print postfix expression
    printf("Postfix expression: %s\n", PRE);

    return PRE;
}


TreeNode* stack_ET(char* postfixExp) {
  // preConverter(postfixExp);
       StackNode* stack = NULL;
       //for loop to the postfix expr in a char array to make the operation on it
    for (int i = 0; postfixExp[i] != '\0'; ++i) {
        char ch = postfixExp[i];
        //to insert the operater in stack and make it as tree node , and if the operatrer is multiple put it in the tree as it is like 10,11,.....
        if (isdigit(ch)) {
            int len = 0;
            while (isdigit(postfixExp[i])) {
                len++;
                i++;
            }
            i--;

            char* num = (char*)malloc((len + 1) * sizeof(char));
            strncpy(num, &postfixExp[i - len + 1], len);
            num[len] = '\0';
            //  to but the operant in a tree node and the left and rghit always null
            TreeNode* newNode = (TreeNode*)malloc(sizeof(TreeNode));
            newNode->data = num;
            newNode->left = newNode->right = NULL;
            push(&stack, newNode);
        }
        // if the ch is an operation create a tree node and put the operation and the right of it is fierst pop the left second pop then puch it to the stack
         else if (isOperator(ch)) {
            TreeNode* newNode = (TreeNode*)malloc(sizeof(TreeNode));
            newNode->data = ch;
            newNode->right = pop(&stack);
            newNode->left = pop(&stack);
            push(&stack, newNode);
        }
    }
// return all tree
    TreeNode* root = pop(&stack);
    return root;
}

// function to evaluate the expr by tree
int calu_tree(TreeNode*tree){
   // stack_ET(tree);

    if(!tree)
        return 0;
        //leef node
    if (!tree->left && !tree->right){
        return atoi(tree->data);
    }

    int val1=calu_tree(tree->rigth);
    int val2=calu_tree(tree->left);

    if (tree->data=='+')
    return val1 + val2;

     if (tree->data=='-')
    return val1 - val2;

     if (tree->data=='*')
    return val1 * val2;

     if (tree->data=='/'){
            //we can not dvide numbers by 0
            if (val2 == 0) {
            printf("Error: Division by zero.\n");
            exit(EXIT_FAILURE);
        }
        return val1 / val2;
    }


     if (tree->data=='%'){
            if (val2 == 0) {
            printf("Error: Modulo by zero.\n");
            exit(EXIT_FAILURE);
        }
        return val1 % val2;
    }


}


// this function have two fuunction first to create the tree and the second to cal the tree
int evaluatePostfix(char* postfixExp) {
    TreeNode* root = stack_ET(postfixExp);
    int result = calu_tree(root);
    return result;
}


// Function to load strings from a file and insert into the linked list
void loadFromFile(struct NODE* head, const char* filename)
{
    FILE* file = fopen(filename , "r");
    if (file == NULL)
    {
        printf("Error: Could not open file %s\n", filename);
        return;
    }

    char buffer[31];

    // Read strings from the file and insert into the linked list
    while (fgets(buffer, sizeof(buffer), file) != NULL)
    {
        // Remove newline character if present
        size_t len = strlen(buffer);
        if (buffer[len - 1] == '\n')
        {
            buffer[len - 1] = '\0';
        }

        // Insert the string into the linked list
        Insert_At_End(buffer, head);
    }

    fclose(file);
}



// to print menu
void print_menu (){
printf("1. Read	equations\n");
printf("2. Print equations\n");
printf("3. Evaluate using Expression tree\n");
printf("4. Print postfix expressions\n");
printf("5. Save	to output file (postfix and results)\n");
printf("6. Exit\n");
}


int main() {
    printf("                                   Calculate postfix by using Expression tree                                  \n ");
    printf("_______________________________________________________________________________________________________________________");
    struct NODE* equationList = createlist(); // Create a linked list to store equations
    struct NODE* current2 = createlist();
    int choice;

    do {
        // Display menu
        printf("\n");
        printf("-----------------------------------------");
        printf("\n Menu: \n");
        print_menu();
         printf("-----------------------------------------");
        printf("\n");

        printf("\nEnter your choice: ");

        scanf("%d", &choice);
        getchar(); // Consume newline character

        switch (choice) {
            case 1: {
                // Read equations from a file
                loadFromFile(equationList, "input.txt");
                printf("\n");
                printf("Equations read from file successfully.\n");
                break;
            }
            case 2:
                // Print equations
                printf("\n");
                printf("Equations:\n");
                struct NODE* current = equationList->NEXT; // Skip the dummy node
                while (current != NULL) {
                    printf("%s\n", current->string);
                    current = current->NEXT;
                }
                break;
            case 3:
                // Evaluate using Expression tree
                printf("\n");
                printf("Evaluating using Expression tree:\n");
                current2 = equationList->NEXT;
                while (current2 != NULL) {
                   char * m =preConverter(current2->string);
                    int result = evaluatePostfix(m);
                    printf("Result: %d\n", result);

                    current2 = current2->NEXT;
                }
                break;
            case 4:
                // Print postfix expressions
                printf("\n");
                printf("Postfix expressions:\n");
                current = equationList->NEXT; // Skip the dummy node
                while (current != NULL) {
                    preConverter(current->string);
                    current = current->NEXT;
                }
                break;
            case 5: {
                // Save to output file (postfix and results)
                char outputFilename[MAX] = "output.txt";


                FILE* outputFile = fopen(outputFilename, "w");
                if (outputFile == NULL) {
                    printf("Error: Could not open output file %s\n", outputFilename);
                    break;
                }

                current = equationList->NEXT; // Skip the dummy node
                while (current != NULL) {
                    // Print postfix expression to file
                    fprintf(outputFile, "Postfix expression for '%s': ", current->string);
                    preConverter(current->string);
                    fprintf(outputFile, "\n");

                    // Evaluate using Expression tree and print result to file
                    TreeNode* expressionTree = stack_ET(current->string);
                    fprintf(outputFile, "Result: %d\n", calu_tree(expressionTree));

                    freeTree(expressionTree); // Free the expression tree

                    current = current->NEXT;
                }

                fclose(outputFile);
                printf("Results saved to %s successfully.\n", outputFilename);
                break;
            }
            case 6:
                printf("Exiting the program.\n");
                break;
            default:
                printf("Invalid choice. Please enter a valid option.\n");
        }
    } while (choice != 6);

    return 0;
}








