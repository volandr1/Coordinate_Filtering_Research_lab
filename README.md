# Лабораторна робота: Обробка координатних даних: Придушення шумів у потоці (Real-time)

**Автор:** Волошин Андрій Юрійович

## 1. Реалізація (Implementation)

У цій роботі реалізовано програмну архітектуру для потокової обробки координатних даних, що імітує роботу мікроконтролера в реальному часі. Було імплементовано три типи цифрових фільтрів:

### Методи `update`:

```python
# SMA (Simple Moving Average) - Реалізація з оптимізацією O(1)
def update(self, x):
    # Якщо черга повна, віднімаємо старий елемент від загальної суми
    if len(self.q) == self.w:
        self.sum -= self.q[0]
    # Додаємо новий елемент у чергу та до суми
    self.q.append(x)
    self.sum += x
    # Повертаємо середнє значення
    return self.sum / len(self.q)

# EMA (Exponential Moving Average) - Рекурсивний фільтр
def update(self, x):
    # Якщо це перший відлік, ініціалізуємо значенням x
    if self.last is None:
        self.last = x
    else:
        # Рекурсивна формула: alpha * x + (1 - alpha) * last
        self.last = self.a * x + (1 - self.a) * self.last
    return self.last

# Median Filter - Нелінійний фільтр для ігнорування викидів
def update(self, x):
    # Додаємо x у чергу
    self.q.append(x)
    # Повертаємо медіану поточного вікна
    return np.median(self.q)
```
