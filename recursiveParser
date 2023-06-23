'''
This program implements a recursive descent parser for the CFG below:

The grammar has added pi and unary minus to the previous program.
Also, the parse function is now called in a loop, so you can evaluate
one expression after another.
------------------------------------------------------------
1 <exp> → <term>{+<term> | -<term>}
2 <term> → <factor>{*<factor> | /<factor>}
3 <factor> → <number> | pi | -<factor>
'''
import math

class ParseError(Exception): pass

#==============================================================
# FRONT END PARSER
#==============================================================

i = 0 # keeps track of what character we are currently reading.
err = None
#---------------------------------------
# Parse an Expression   <exp> → <term>{+<term> | -<term>}
#
def exp():
    global i, err

    value = term()
    while True:
        if w[i] == '+':
            i += 1
            value = binary_op('+', value, term())
        elif w[i] == '-':
            i += 1
            value = binary_op('-', value, term())
        else:
            break

    return value
#---------------------------------------
# Parse a Term   <term> → <factor>{+<factor> | -<factor>}
#
def term():
    global i, err

    value = factor()
    while True:
        if w[i] == '*':
            i += 1
            value = binary_op('*', value, factor())
        elif w[i] == '/':
            i += 1
            value = binary_op('/', value, factor())
        else:
            break

    return value
#---------------------------------------
# Parse a Factor   <factor> → (<exp>) | <number> 
#       
def factor():
    global i, err
    value = None
    
    if w[i] == 'pi':
        i += 1
        return math.pi
    elif w[i] == '-':
        i += 1
        return unary_op('-', factor())
    elif w[i] == '(':
        i+=1
        value = exp()
        if w[i] == ')':
            i+=i
            return value
        else:
            print("Missing )")
            raise ParseError
    else:
        try:
            value = atomic(w[i])
            i += 1          # read the next character
        except ValueError:
            print('number expected')
            value = None
    
    #print('factor returning', value)
    
    if value == None: raise ParseError
    return value


#==============================================================
# BACK END PARSER (ACTION RULES)
#==============================================================

def binary_op(op, lhs, rhs):
    if op == '+': return lhs + rhs
    elif op == '-': return lhs - rhs
    elif op == '*': return lhs * rhs
    elif op == '/': return lhs / rhs
    else: return None

def unary_op(op, rhs):
    if op == '-': return -rhs
    else: return None

def atomic(x):
    return float(x)

def assign(t):



#==============================================================
# User Interface Loop
#==============================================================
w = input('\nEnter expression: ')
while w != '':
    #------------------------------
    # Split string into token list.
    #
    for c in '()+-*/':
        w = w.replace(c, ' '+c+' ')
    w = w.split()
    w.append('$') # EOF marker

    print('\nToken Stream:     ', end = '')
    for t in w: print(t, end = '  ')
    print('\n')
    i = 0
    try:
        print('Value:           ', exp()) # call the parser
    except:
        print('parse error')
    print()
    if w[i] != '$': print('Syntax error:')
    print('read | un-read:   ', end = '')
    for c in w[:i]: print(c, end = '')
    print(' | ', end = '')
    for c in w[i:]: print(c, end = '')
    print()
    w = input('\n\nEnter expression: ')
class Node:
    def __init__(self, data, left = None, right = None):
        self.left = left
        self.right = right
        self.data = data
        # Dictionary to translate operators to function names
        fun = {'+':'ADD', '-':'SUB','*':'MUL','/':'DIV'}
        #-----------------------------------------------------------------------------#
        #Functions to print Abtract Syntax Tree (AST) in various formats.
        #
        def Prefix(n):
            if n.left == None and n.right == None:
                return n.data
            if n.data in binary_op:
                return str(n.data) + ' ' + Prefix(n.left) + ' ' + Prefix(n.right)
        def Lisp(n):
            if n.left == None and n.right == None:
                return n.data
            if n.data in binary_op:
                return '(' + str(n.data) + ' ' + Lisp(n.left) + ' ' + Lisp(n.right) + ')'
        def RPN(n):
            if n.left == None and n.right == None:
                return n.data
            if n.data in binary_op:
                return RPN(n.left) + ' ' + RPN(n.right) + ' '+ str(n.data)
        def Func(n):
            if n.left == None and n.right == None:
                return n.data
            if n.data == binary_op:
                return fun[n.data] + '(' + Func(n.left) + ', ' + Func(n.right) + ')'
        def Stack(n):
            if n.left == None and n.right == None:
                return 'LOAD ' + n.data + '\n'
            return  Stack(n.left) + Stack(n.right) + fun[n.data] + '\n'
                #-----------------------------------------------------------------------------# Function to build AST from a prefix expression.
                # Operators and values must be seperated by spaces, ie:# the prefix expression:
                # * 2 + 1 * + 34 45 - 50 22
                # is equivalent to the infix expression:
                # 2*(1 +(34 + 45)*(50 - 22))#
        def buildPrefix(e): # calls the recursive parser
            i = 0               # read position in token stream
            t = e.split()       # create token list
            def buildPrefix():  # recursive parser
                nonlocal i
                token = t[i]
                i += 1          # advance read position
                if token in '+-*/': # token is an operator
                    return Node(token, buildPrefix(), buildPrefix())
                else:               # token is a value
                    return Node(token)
            return buildPrefix()# Use the insert method to add nodes
            p = input('Enter prefix expression: ')
        root = buildPrefix(p)
        print()
        print(Lisp(root))
        print(Prefix(root))
        print(RPN(root))
        print(Func(root))
        print()
        print(Stack(root))
#print(w[:i], '|', w[i:])
