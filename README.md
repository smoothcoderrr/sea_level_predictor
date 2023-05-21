# sea_level_predictor
import pandas as pd
import seaborn as sns
from sklearn.linear_model import LinearRegression
import matplotlib.pyplot as plt

data=pd.read_csv("epa-sea-level_csv.csv",header=0)
data.head()

data.describe()

data.info()

del data["NOAA Adjusted Sea Level"]
data=data.dropna()

data.info()

data["Year"]=pd.to_datetime(data["Year"])
data["Year"]=data.Year.dt.year
data.Year.head()

lm=LinearRegression()
lm.fit(data[["Year"]],data["CSIRO Adjusted Sea Level"])
intercept=lm.intercept_
slope=lm.coef_
print(intercept,slope)

line_x=range(1880,2000)
line_y=intercept+(slope*line_x)
plt.plot(line_x,line_y)
plt.xlabel("YEAR")
plt.ylabel("RISE IN SEA LEVEL")
plt.show()

recent_data=data[(data["Year"]>=2000)]
line_x1=range(2000,2051)
lm.fit(recent_data[["Year"]],recent_data["CSIRO Adjusted Sea Level"])
recent_intercept=lm.intercept_
recent_slope=lm.coef_
print(recent_intercept,recent_slope)

y_recent_predict=recent_intercept+(recent_slope*line_x1)
plt.plot(line_x1,y_recent_predict)
plt.xlabel("YEAR")
plt.ylabel("RISE IN SEA LEVEL")
plt.show()

Y_increase=recent_intercept+(recent_slope*2050)
print("the increase in sealevel in 2050 is",Y_increase)
