# Ex.No: 02 LINEAR AND POLYNOMIAL TREND ESTIMATION
Date:02-05-2026
## NAME : G P HARIESH
## REG.NO : 212224040100

### AIM:
To Implement Linear and Polynomial Trend Estiamtion Using Python.
## REQUIREMENTS 
Dataset : salesdata.csv
Software : google colab

### ALGORITHM:
Import necessary libraries (NumPy, Matplotlib)

Load the dataset

Calculate the linear trend values using least square method

Calculate the polynomial trend values using least square method

End the program
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

data = pd.read_csv("sales_data.csv")

data['Sale_Date'] = pd.to_datetime(data['Sale_Date'])

data.set_index('Sale_Date', inplace=True)


resampled_data = data['Unit_Price'].resample('ME').sum().to_frame()


resampled_data.reset_index(inplace=True)
resampled_data['Period'] = resampled_data['Sale_Date'].dt.strftime('%Y-%m')

print(resampled_data.head())


periods = resampled_data['Period'].tolist()
price = resampled_data['Unit_Price'].tolist()


X = list(range(-(len(periods)//2), len(periods) - (len(periods)//2)))


x2 = [i**2 for i in X]
xy = [i*j for i, j in zip(X, price)]

n = len(X)

b = (n * sum(xy) - sum(price) * sum(X)) / (n * sum(x2) - (sum(X)**2))
a = (sum(price) - b * sum(X)) / n

linear_trend = [a + b * X[i] for i in range(n)]


x3 = [i**3 for i in X]
x4 = [i**4 for i in X]
x2y = [i*j for i, j in zip(x2, price)]

coeff = [
    [n, sum(X), sum(x2)],
    [sum(X), sum(x2), sum(x3)],
    [sum(x2), sum(x3), sum(x4)]
]

Y = [sum(price), sum(xy), sum(x2y)]

A = np.array(coeff)
B = np.array(Y)

solution = np.linalg.solve(A, B)
a_poly, b_poly, c_poly = solution

poly_trend = [a_poly + b_poly*X[i] + c_poly*(X[i]**2) for i in range(n)]

print(f"Linear Trend: y = {a:.2f} + {b:.2f}x")
print(f"Polynomial Trend: y = {a_poly:.2f} + {b_poly:.2f}x + {c_poly:.2f}x²")

resampled_data['Linear Trend'] = linear_trend
resampled_data['Polynomial Trend'] = poly_trend

plt.figure(figsize=(14,6))

plt.plot(resampled_data['Period'], resampled_data['Unit_Price'],
         marker='o', label='Actual Unit Price')

plt.plot(resampled_data['Period'], resampled_data['Linear Trend'],
         linestyle='--', label='Linear Trend')

plt.plot(resampled_data['Period'], resampled_data['Polynomial Trend'],
         marker='o', label='Polynomial Trend')

plt.title("Monthly Unit Price Trend")
plt.xlabel("Month")
plt.ylabel("Unit_Price")
plt.xticks(rotation=45)
plt.legend()
plt.grid(True)

plt.tight_layout()
plt.show()
```

### OUTPUT

<img width="1034" height="606" alt="image" src="https://github.com/user-attachments/assets/9a15a26d-e856-47ef-9523-8eb29c001e18" />


### RESULT:
Thus the python program for linear and Polynomial Trend Estiamtion has been executed successfully.
