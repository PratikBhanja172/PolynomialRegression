import numpy as np
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

df = pd.read_csv("/content/FuelConsumptionCo2.csv")

df.head()

X = df[['ENGINESIZE', 'CYLINDERS', 'FUELCONSUMPTION_CITY', 'FUELCONSUMPTION_HWY', 'FUELCONSUMPTION_COMB']].values  # Independent variables
y = df[['CO2EMISSIONS']].values  # Dependent variable

from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

poly = PolynomialFeatures(degree=2)
X_train_poly = poly.fit_transform(X_train)
X_test_poly = poly.transform(X_test)

# Training Polynomial Regression Model
model = LinearRegression()
model.fit(X_train_poly, y_train)

y_pred = model.predict(X_test_poly)

r2 = r2_score(y_test, y_pred)

r2

fig = plt.figure(figsize=(10, 7))
ax = fig.add_subplot(111, projection='3d')
ax.scatter(X_test[:, 0], X_test[:, 1], y_test, color='blue', label="Actual")  # Actual values
ax.scatter(X_test[:, 0], X_test[:, 1], y_pred, color='red', label="Predicted")  # Predicted values
ax.set_xlabel('Engine Size')
ax.set_ylabel('Cylinders')
ax.set_zlabel('CO2 Emissions')
ax.set_title('3D Visualization of Polynomial Regression')
ax.legend()
plt.show()

class MerePolyLR:
    def __init__(self, degree=2):
        self.degree = degree
        self.coef_ = None
        self.intercept_ = None
        self.poly = PolynomialFeatures(degree=self.degree)

    def fit(self, x_train, y_train):
        x_train_poly = self.poly.fit_transform(x_train)
        betas = np.linalg.inv(x_train_poly.T.dot(x_train_poly)).dot(x_train_poly.T).dot(y_train)
        self.intercept_ = betas[0]
        self.coef_ = betas[1:]

    def predict(self, x_test):
        x_test_poly = self.poly.transform(x_test)
        return np.dot(x_test_poly, np.append(self.intercept_, self.coef_))



X_data = df[['ENGINESIZE', 'CYLINDERS', 'FUELCONSUMPTION_CITY', 'FUELCONSUMPTION_HWY', 'FUELCONSUMPTION_COMB']]
y_data = df[['CO2EMISSIONS']]

lr = LinearRegression()
lr.fit(X_data, y_data)

y_pred = lr.predict(X_test)

y_pred

r2 = r2_score(y_test, y_pred)

print("r2 is :",r2)

