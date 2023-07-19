---
sidebar_position: 10
---

# 🔴 Калібрування ВММ

Можна протидіяти деяким упередженням ВММ, якщо відкалібрувати **вихідні розподіли**(@zhao2021calibrate).

**Що саме означає відкалібрувати вихідний розподіл?**

Розглянемо короткий приклад: припустимо, у нас є %%s аналіз тональності тексту|завдання на аналіз тональності тексту%% із двома можливими мітками: `Positive` і `Negative`. Уявіть, що відбувається, коли %%ВММ|ВММ%% отримує запит `Input: nothing Sentiment:` (Введення: нічого Тональність). Це введення не містить жодного _контексту_, який ВММ може використовувати, щоб передбачити тональність, тому його називають **контекстно-вільним** введенням.

Оскільки `nothing` (нічого) є ні позитивним, ні негативним поняттям, ми очікуємо, що ВММ виведе ймовірність приблизно 0,5 для `Positive` і `Negative`. Однак часто (і для цього прикладу) це не так.
```
p("Positive" | "Input: nothing Sentiment:") = 0,9

p("Negative" | "Input: nothing Sentiment:") = 0,1
```

Враховуючи ці ймовірності міток для контекстно-вільних вхідних даних, ми знаємо, що розподіл **вихідних даних** ВММ, ймовірно, упереджений до мітки `Positive`. Це може призвести до того, що ВММ віддасть перевагу `Positive` для всіх вхідних даних, навіть якщо вхідні дані насправді не є позитивними.

Якщо ми можемо якимось чином **відкалібрувати** вихідний розподіл так, щоб безконтекстні введення отримали ймовірність 0,5 як для `Positive`, так і для `Negative`, тоді ми часто можемо усунути упередження до `Positive` і ВММ буде більш надійною як для безконтекстних введень, так і для введень із контекстом.

## Нетехнічне рішення

Нетехнічне розв'язання цієї проблеми полягає в тому, щоб просто надати кілька типових прикладів, де безконтекстним зразкам фактично призначається ймовірність 0,5 як для `Positive`, так і для `Negative`.

Наприклад, ми могли б навести кілька типових прикладів, які показують, що кожен безконтекстний екземпляр класифікується як `Positive` і `Negative`:
```
Input: Я ненавиджу цей фільм. Sentiment: Negative
Input: Мені подобається цей фільм. Sentiment: Positive
Input: N/A Sentiment: Positive
Input: N/A Sentiment: Negative
Input: nothing Sentiment: Positive
Input: nothing Sentiment: Negative
Input: Мені подобаються яйця. Sentiment:
```

Наскільки мені відомо, в науковій літературі не має досліджень на тему цього рішення, і я не впевнений, наскільки добре воно працює на практиці. Однак це просте рішення, яке демонструє, чого ми намагаємось досягти калібруванням.

## Технічне рішення

Іншим вирішенням цього є __контекстне калібрування__(@zhao2021calibrate), де ми налаштовуємо спеціальні параметри калібрування, які гарантують, що безконтекстним введенням, таким як `Input: nothing Sentiment:` (введення: нічого Тональність) призначається ймовірність приблизно 0,5 для обох міток. Зауважте, що на практиці цей метод виконує калібрування за кількома різними безконтекстними введеннями (наприклад, `Input: N/A Sentiment:`, `Input: [MASK] Sentiment:`). Він усереднює параметри калібрування, які найкраще працюють для кожного контекстно-вільного введення, щоб знайти найкращі параметри калібрування для ВММ.

### Приклад

Розгляньмо приклад обчислення параметрів калібрування для одного безконтекстного введення. Зауважте, що цей приклад не можна відтворити за допомогою GPT-3 через те, що його не можна обмежити мітками `Positive` і `Negative`.

Знову розглянемо наведений вище приклад, де ВММ призначає такі ймовірності міткам для безконтекстних введень:

```
p("Positive" | "Input: nothing Sentiment:") = 0.9

p("Negative" | "Input: nothing Sentiment:") = 0.1
```

Ми хочемо знайти такий розподіл ймовірностей q, щоб
```
q("Positive" | "Input: nothing Sentiment:") = 0.5

q("Negative" | "Input: nothing Sentiment:") = 0.5
```

Ми зробимо це, створивши лінійне перетворення, яке коригує (калібрує) ймовірності $p$.

$\hat q = \text{Softmax}(W\hat p + b)$

Це рівняння бере вихідні ймовірності $\hat p$ й застосовує ваги $W$ й упередження $b$ до них. Ваги $W$ та упередження $b$ є параметрами калібрування, які, при застосуванні до ймовірності безконтекстного прикладу дадуть $\hat p$ = [0,5, 0,5].

#### Обчислення W і b

Нам потрібно якось обчислити ваги $W$ та упередження $b$. Один зі способів зробити це:

$W = \text{diag}(\hat p)^{-1}$

$b = 0 $

Хоча спочатку визначення $W$ може здатися трохи дивним, але воно просто бере обернене кожне значення в $\hat p$, щоб знайти $W$ , який перетворить вихідні ймовірності $\hat p$ у калібровані ймовірності [0,5, 0,5].

Перевірмо, що це працює для прикладу вище:

$\hat p = [0,9, 0,1]$

$W = \text{diag}(\hat p)^{-1} = \text{diag}([0,9, 0,1])^{-1} = \begin{bmatrix}    0,9 & 0 \\
   0 & \end{bmatrix}^{-1} = \begin{bmatrix}    1,11 & 0 \\
   0 & 10 \end{bmatrix}$

$\hat q = \text{Softmax}(W\hat p + b) = \text{Softmax}(\begin{bmatrix}    1.11 & 0 \\
   0 & 10 \end{bmatrix}*{[0,9, 0,1]} + 0) = \text{Softmax}([1, 1]) =[0,5, 0,5]$

Як згадувалося вище, ми виконували цей процес для кількох різних безконтекстних введень і отримали середні значення параметрів калібрування, які найкраще працюють для кожного контекстно-вільного введення, щоб знайти найкращі параметри калібрування для ВММ. Це означає, що остаточні параметри калібрування, ймовірно, не будуть показувати будь-які безконтекстні введення точно [0,5, 0,5].

### Інший метод

$b$ також можна встановити на $-\hat p$, та $W$ — на ідентифікаційну матрицю. Цей метод виконує кращі завдання генерації, ніж класифікації (@zhao2021calibrate).

## Висновки

ВММ часто схильні (упереджені) до певних ярликів. Калібрування може бути використане для протидії цьому упередженню.