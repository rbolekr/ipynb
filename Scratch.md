import random
from sympy import *
from io import StringIO
import sys

categories = ['alpha', 'beta', 'gamma']
selected_category = random.choices(categories, weights=[0.2, 0.4, 0.4])[0]
x, y = symbols('x, y')
f = None
solution = None

selected_category = categories[0]

if selected_category == 'alpha':
    a = random.randint(1, 20)
    b = random.randint(1, 7)
    c = random.randint(1, 20)
    d = random.randint(2, 4)
    e = random.randint(1, 20)
    f = random.randint(1, 20)
    
    sign_a = random.choice(['', '-'])
    sign_c = random.choice(['', '-'])
    sign_d = random.choice(['+', '-'])
    sign_e = random.choice(['+', '-'])
    
    f = parse_expr(f"{sign_a}{a}*x**{b}*({sign_c}{c}*x**{d}{sign_d}{e}*x{sign_e}{f})")
    
elif selected_category == 'beta':
    f = x**3 - 3*x*y + y**3
    
else:
    f = x**4 + 4*x**3*y + 6*x**2*y**2 + 4*x*y**3 + y**4

#bool(f1.expand() == f2.expand())
#simplify(f1-f2) == 0

solution = f.expand()


old_stdout = sys.stdout
sys.stdout = buffer = StringIO()
latex(f)
sys.stdout = old_stdout

latex_representation = buffer.getvalue().strip()
print(latex_representation)


def click_handler(button):
    value = interface.answer_text.value.strip()
    try:
        answer_int_value = int(value)
        if answer_int_value == solution:
            print(f'Correct! Please reload for another question.')
        else:
            print(f'{answer_int_value} is incorrect. Try again or peak at the other tab!')
    except ValueError:
        print(f'"{value}" does not seem like a number. Try again or peak at the other tab!')

interface = Interface(
    question_label_value=f'Expand {f}?',
    answer_label_value=f'{solution}',
    check_button_click_handler=click_handler
)

#interface.display()