
```
# Load data
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'

melbourne_data = pd.read_csv(melbourne_file_path) 

# Filter rows with missing price values
filtered_melbourne_data = melbourne_data.dropna(axis=0)

# Choose target and features
y = filtered_melbourne_data.Price
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'BuildingArea', 
                        'YearBuilt', 'Lattitude', 'Longtitude']
X = filtered_melbourne_data[melbourne_features]

# Split data into test and val
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0)

# Define model
melbourne_model = DecisionTreeRegressor()

# Fit model
melbourne_model.fit(train_X, train_y)

# Get predicted prices on validation data
val_predictions = melbourne_model.predict(val_X)

# Get MAE to determine effectiveness
print(mean_absolute_error(val_y, val_predictions))
```

