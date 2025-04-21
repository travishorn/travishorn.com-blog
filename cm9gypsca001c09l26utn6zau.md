---
title: "Real-Time Delivery ETA Prediction in Python"
datePublished: Mon Apr 14 2025 11:00:20 GMT+0000 (Coordinated Universal Time)
cuid: cm9gypsca001c09l26utn6zau
slug: real-time-delivery-eta-prediction-in-python
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1745024792544/6bf2d314-493c-4c6d-9d57-766a894d512d.png
tags: realtime, python, machine-learning, predictive-analysis, delivery-management-system

---

I’ve been playing around with how ML techniques can be applied to logistics and e-commerce. One of the ideas I had floating around my head was to build a model that can be trained on delivery data and then used in real-time as delivery data comes in to do things like predict ETAs. In the real world, things like traffic jams and sudden changes in weather can drastically affect the last mile of delivery. A real-time system that can provide up-to-the-second updates on the ETA of a delivery would be highly beneficial here.

I ended up coming up with this **Delivery ETA Prediction System**. It’s is a proof-of-concept that uses machine learning to predict and dynamically adjust delivery ETAs in real-time. It simulates delivery drivers, incorporating factors like location, speed, and environmental conditions, to provide more accurate and up-to-date arrival estimates.

Let’s dive into how this system works. We’ll go over the architecture, the key components, and the technologies used. We'll explore how the system simulates deliveries, how it trains a machine learning model to predict ETAs, and how it uses that model to provide real-time updates. For a basic system like this, it’s easier to understand than you might think. By the end, you'll understand how such a system can be built and the potential it has for improving delivery accuracy.

## Overview

So, what does this system actually *do*? At its core, the project simulates the journey of multiple delivery drivers and uses a machine learning model to predict their arrival times. It doesn't just make a single prediction at the start. Instead, it continuously monitors the simulated real-time data stream and adjusts the ETAs based on the latest information.

The main goal was to create a simple, functional POC demonstrating how ML can be used for dynamic and realistic delivery estimates compared to static calculations.

Here are the key features:

* **Real-Time Data Simulation:** It generates a stream of mock data mimicking actual delivery scenarios, including driver locations, speeds, status changes (like delays), and varying environmental conditions (traffic, weather).
    
* **Machine Learning Prediction:** It employs a Random Forest machine learning model, trained on the simulated historical data, to predict the remaining travel time.
    
* **Dynamic ETA Adjustments:** As new data points come in from the simulator, the system recalculates the ETA, taking into account current factors like distance remaining, current speed, time of day, traffic levels, and weather.
    
* **Real-Time Status Updates:** It provides immediate feedback on whether a delivery is projected to be "ON TIME", "DELAYED", or "EARLY" compared to its original estimate.
    

## The Code

The entire system hinges on three core Python scripts working together. Let's break them down one by one, starting with the foundation: the data simulator.

### Real-Time Data Simulator (`simulator.py`)

Machine learning models need data, and for a real-time system, we need a *stream* of data. Since we don't have access to a fleet of actual delivery drivers, we need to simulate them! That's the job of `simulator.py`.

```python
import json
import random
import time
from datetime import datetime, timedelta
from typing import Dict, List, Tuple
from geopy.distance import geodesic

class SimulationClock:
    def __init__(self, time_acceleration_factor: float = 1.0):
        """Initialize simulation clock with optional time acceleration.
        
        Args:
            time_acceleration_factor: How much faster than real-time to run (e.g., 60.0 = 1 minute per second)
        """
        self.time_acceleration_factor = time_acceleration_factor
        self.start_real_time = time.time()
        self.start_sim_time = datetime.now()
    
    def now(self) -> datetime:
        """Get current simulation time."""
        elapsed_real_seconds = time.time() - self.start_real_time
        elapsed_sim_seconds = elapsed_real_seconds * self.time_acceleration_factor
        return self.start_sim_time + timedelta(seconds=elapsed_sim_seconds)
    
    def sleep(self, seconds: float):
        """Sleep for the specified number of simulation seconds."""
        real_seconds = seconds / self.time_acceleration_factor
        time.sleep(real_seconds)

class Driver:
    def __init__(self, driver_id: str, start_location: Tuple[float, float], 
                 destination_location: Tuple[float, float], initial_speed_mph: float = 40.0):
        self.driver_id = driver_id
        self.start_location = start_location
        self.destination_location = destination_location
        self.current_location = start_location
        self.current_speed_mph = initial_speed_mph
        self.status = 'EN_ROUTE'
        self.initial_planned_eta = None
        self.delay_end_time = None
        
    def calculate_distance_remaining(self) -> float:
        """Calculate remaining distance in miles."""
        return geodesic(self.current_location, self.destination_location).miles
    
    def is_completed(self) -> bool:
        """Check if delivery is completed (within 0.1 mi of destination)."""
        return self.status == 'COMPLETED'

class DeliverySimulator:
    def __init__(self, num_drivers: int = 3, update_interval: int = 10, time_acceleration_factor: float = 1.0):
        """Initialize the delivery simulator.
        
        Args:
            num_drivers: Number of drivers to simulate
            update_interval: How often to update driver states (in simulation seconds)
            time_acceleration_factor: How much faster than real-time to run (e.g., 60.0 = 1 minute per second)
        """
        self.drivers: List[Driver] = []
        self.update_interval = update_interval
        self.weather_conditions = ['CLEAR', 'RAIN', 'SNOW']
        self.traffic_levels = ['LOW', 'MEDIUM', 'HIGH']
        self.clock = SimulationClock(time_acceleration_factor)
        
        # Initialize drivers with random start/end locations
        self._initialize_drivers(num_drivers)
        
    def _initialize_drivers(self, num_drivers: int):
        """Initialize drivers with random start/end locations within a reasonable area."""
        # Using Chicago area as an example boundary
        bounds = {
            'min_lat': 41.7,
            'max_lat': 42.0,
            'min_lon': -87.9,
            'max_lon': -87.5
        }
        
        for i in range(num_drivers):
            start_location = (
                random.uniform(bounds['min_lat'], bounds['max_lat']),
                random.uniform(bounds['min_lon'], bounds['max_lon'])
            )
            destination_location = (
                random.uniform(bounds['min_lat'], bounds['max_lat']),
                random.uniform(bounds['min_lon'], bounds['max_lon'])
            )
            
            driver = Driver(f"D{i+1}", start_location, destination_location)
            # Set initial ETA based on distance and initial speed
            distance = driver.calculate_distance_remaining()
            initial_time_hours = distance / driver.current_speed_mph
            driver.initial_planned_eta = self.clock.now() + timedelta(hours=initial_time_hours)
            self.drivers.append(driver)
    
    def _update_driver_status(self, driver: Driver):
        """Update driver status with random events."""
        if driver.delay_end_time and self.clock.now() >= driver.delay_end_time:
            driver.status = 'EN_ROUTE'
            driver.current_speed_mph = random.uniform(35.0, 45.0)
            driver.delay_end_time = None
            return
        
        if driver.status == 'EN_ROUTE' and random.random() < 0.1:  # 10% chance of event
            if random.random() < 0.5:  # 50% chance of traffic vs stop
                driver.status = 'DELAYED_TRAFFIC'
                driver.current_speed_mph *= 0.3  # Reduce speed significantly
            else:
                driver.status = 'AT_STOP'
                driver.current_speed_mph = 0
            
            # Set delay duration between 2-5 minutes
            delay_duration = random.uniform(2, 5)
            driver.delay_end_time = self.clock.now() + timedelta(minutes=delay_duration)
    
    def _update_driver_location(self, driver: Driver):
        """Update driver location based on current speed and heading."""
        if driver.status == 'AT_STOP' or driver.is_completed():
            return
            
        # Calculate movement vector
        distance_mi = (driver.current_speed_mph * self.update_interval) / 3600  # Convert to mi/update
        total_distance = geodesic(driver.current_location, driver.destination_location).miles
        
        # If we're very close to destination or would overshoot, complete the delivery
        if total_distance <= 0.05 or distance_mi >= total_distance:
            driver.current_location = driver.destination_location
            driver.status = 'COMPLETED'
            driver.current_speed_mph = 0
            return
            
        # Linear interpolation for normal movement
        fraction = distance_mi / total_distance
        new_lat = driver.current_location[0] + (driver.destination_location[0] - driver.current_location[0]) * fraction
        new_lon = driver.current_location[1] + (driver.destination_location[1] - driver.current_location[1]) * fraction
        driver.current_location = (new_lat, new_lon)
    
    def _generate_data_point(self, driver: Driver) -> Dict:
        """Generate a data point for the current driver state."""
        current_time = self.clock.now()
        return {
            "timestamp": current_time.isoformat(),
            "driver_id": driver.driver_id,
            "current_latitude": driver.current_location[0],
            "current_longitude": driver.current_location[1],
            "current_speed_mph": driver.current_speed_mph,
            "destination_latitude": driver.destination_location[0],
            "destination_longitude": driver.destination_location[1],
            "status": driver.status,
            "traffic_level": random.choice(self.traffic_levels),
            "weather": random.choice(self.weather_conditions),
            "initial_planned_eta": driver.initial_planned_eta.isoformat() if driver.initial_planned_eta else None
        }
    
    def run(self):
        """Main simulation loop."""
        print(f"Starting delivery simulation (Time acceleration: {self.clock.time_acceleration_factor}x)...")
        print("\nDriver Status Updates:")
        print("-" * 100)  # Separator line
        
        try:
            while True:
                active_drivers = [d for d in self.drivers if d.status != 'COMPLETED']
                if not active_drivers:
                    print("-" * 100)  # Separator line
                    print("All deliveries completed!")
                    break
                
                current_time = self.clock.now()
                for driver in active_drivers:
                    self._update_driver_status(driver)
                    self._update_driver_location(driver)
                    data_point = self._generate_data_point(driver)
                    
                    # Write to output file
                    with open('delivery_data.jsonl', 'a') as f:
                        f.write(json.dumps(data_point) + '\n')
                    
                    # Print status update with fixed-width formatting
                    status_str = f"{driver.status:<15}"  # Left-align status with 15 chars
                    speed_str = f"{driver.current_speed_mph:>6.1f}"  # Right-align speed with 6 chars
                    dist_str = f"{driver.calculate_distance_remaining():>6.2f}"  # Right-align distance with 6 chars
                    
                    print(f"[{current_time.strftime('%Y-%m-%d %H:%M:%S')}] "
                          f"Driver {driver.driver_id:<3} | "  # Left-align driver ID
                          f"Status: {status_str} | "
                          f"Speed: {speed_str} mph | "
                          f"Distance remaining: {dist_str} mi")
                
                self.clock.sleep(self.update_interval)
                
        except KeyboardInterrupt:
            print("\nSimulation stopped by user.")

if __name__ == "__main__":
    # Example: To run 60x faster than real-time (1 minute per second, set time_acceleration_factor=60.0)
    simulator = DeliverySimulator(num_drivers=3, update_interval=10, time_acceleration_factor=1.0)
    simulator.run()
```

This file generates a continuous stream of realistic-looking delivery data points in JSON Lines (`.jsonl`) format. This data serves two purposes:

1. **Training Data:** We run the simulator for a while to generate a historical dataset (`delivery_data.jsonl`) used to train our ML model.
    
2. **Live Data:** Later, we run the simulator alongside the predictor to feed it live updates for real-time ETA calculations.
    

The `SimulationClock` class is clever. It allows the simulation to run faster (or slower) than real-time using a `time_acceleration_factor`. This is super useful for quickly generating lots of training data (e.g., running at 60x speed means 1 minute of simulation passes every second).

The `Driver` class represents each delivery vehicle. It keeps track of essential state information like `driver_id`, current location (latitude/longitude), destination, current speed, status (`EN_ROUTE`, `DELAYED_TRAFFIC`, `AT_STOP`, `COMPLETED`), and the initially planned ETA. It uses the `geopy` library to calculate the distance remaining to the destination.

The `DeliverySimulator` class manages the overall simulation. It initializes a specified number of drivers with random start and end points within a defined geographical area (using Chicago bounds in this example). It calculates an initial ETA based on the straight-line distance and initial speed.

In its main `run` loop, it iteratively updates each active driver:

* It introduces randomness (`_update_driver_status`) – drivers have a chance to get stuck in simulated traffic (slowing down) or make a stop (speed becomes 0) for a random duration.
    
* It moves the drivers towards their destination (`_update_driver_location`). This is simplified using linear interpolation based on their current speed and the time elapsed since the last update. It checks if the driver has reached the destination.
    
* For each driver update, it generates a JSON dictionary (`_generate_data_point`) containing the current timestamp, location, speed, status, destination, randomly chosen traffic/weather conditions, and the original planned ETA.
    
* It appends this JSON data point as a new line to the `delivery_data.jsonl` file and prints a status update to the console.
    
* The loop continues until all drivers reach their destination or the script is stopped.
    

This script effectively creates the dynamic environment our prediction model will learn from and react to.

### The Model Trainer (`train_model.py`)

With a way to generate data, we now need a way to learn from it. That's the role of `train_model.py`.

This file loads the historical delivery data (the `delivery_data.jsonl` file generated by the simulator), processes it, engineers relevant features, and trains a machine learning model to predict the *actual remaining delivery time* in minutes. It also evaluates the model's performance and saves the trained model and necessary preprocessing objects (label encoders) for later use by the real-time predictor.

```python
import json
import pandas as pd
import numpy as np
from datetime import datetime
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error, mean_squared_error, r2_score
import joblib
from typing import Dict, Tuple
from geopy.distance import geodesic

def load_and_preprocess_data(file_path: str) -> pd.DataFrame:
    """Load and preprocess the JSONL data into a pandas DataFrame."""
    # Read JSONL file line by line
    data = []
    with open(file_path, 'r') as f:
        for line in f:
            data.append(json.loads(line))
    
    # Convert to DataFrame
    df = pd.DataFrame(data)
    
    # Convert timestamp and initial_planned_eta to datetime
    df['timestamp'] = pd.to_datetime(df['timestamp'])
    df['initial_planned_eta'] = pd.to_datetime(df['initial_planned_eta'])
    
    # Extract time-based features
    df['hour_of_day'] = df['timestamp'].dt.hour
    df['day_of_week'] = df['timestamp'].dt.dayofweek
    
    # Calculate time-based metrics
    df['time_until_planned_eta'] = (df['initial_planned_eta'] - df['timestamp']).dt.total_seconds() / 60  # in minutes
    
    # Calculate distance remaining
    def calculate_distance(row: pd.Series) -> float:
        return geodesic(
            (row['current_latitude'], row['current_longitude']),
            (row['destination_latitude'], row['destination_longitude'])
        ).miles
    
    df['distance_remaining_mi'] = df.apply(calculate_distance, axis=1)
    
    # Encode categorical variables
    label_encoders = {}
    for col in ['status', 'traffic_level', 'weather']:
        label_encoders[col] = LabelEncoder()
        df[f'{col}_encoded'] = label_encoders[col].fit_transform(df[col])
    
    return df, label_encoders

def prepare_features_and_target(df: pd.DataFrame) -> Tuple[pd.DataFrame, pd.Series]:
    """Prepare feature matrix X and target variable y."""
    features = [
        'distance_remaining_mi',
        'current_speed_mph',
        'hour_of_day',
        'day_of_week',
        'status_encoded',
        'traffic_level_encoded',
        'weather_encoded',
        'time_until_planned_eta'
    ]
    
    X = df[features]
    
    # Target variable: actual remaining time (minutes) to destination
    # Group by driver_id and calculate the time difference to the last record
    df['next_timestamp'] = df.groupby('driver_id')['timestamp'].shift(-1)
    df['actual_remaining_time'] = df.groupby('driver_id')['next_timestamp'].transform('last')
    df['actual_remaining_minutes'] = (df['actual_remaining_time'] - df['timestamp']).dt.total_seconds() / 60
    
    y = df['actual_remaining_minutes']
    
    # Remove any invalid targets (e.g., last points in each trip)
    mask = y.notna()
    
    return X[mask], y[mask]

def train_and_evaluate_model(X: pd.DataFrame, y: pd.Series) -> Tuple[RandomForestRegressor, Dict]:
    """Train the model and evaluate its performance."""
    # Split the data
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
    
    # Initialize and train the model
    model = RandomForestRegressor(
        n_estimators=100,
        max_depth=15,
        min_samples_split=5,
        random_state=42,
        n_jobs=-1
    )
    model.fit(X_train, y_train)
    
    # Make predictions
    y_pred = model.predict(X_test)
    
    # Calculate metrics
    metrics = {
        'mean_absolute_error': mean_absolute_error(y_test, y_pred),
        'root_mean_squared_error': np.sqrt(mean_squared_error(y_test, y_pred)),
        'r2_score': r2_score(y_test, y_pred)
    }
    
    return model, metrics

def main():
    # Load and preprocess the data
    import argparse
    
    print("Loading and preprocessing data...")
    df, label_encoders = load_and_preprocess_data('delivery_data.jsonl')
    
    # Prepare features and target
    print("Preparing features and target...")
    X, y = prepare_features_and_target(df)
    
    # Train and evaluate the model
    print("Training model...")
    model, metrics = train_and_evaluate_model(X, y)
    
    # Print metrics
    print("\nModel Performance:")
    print(f"Mean Absolute Error: {metrics['mean_absolute_error']:.2f} minutes")
    print(f"Root Mean Squared Error: {metrics['root_mean_squared_error']:.2f} minutes")
    print(f"R² Score: {metrics['r2_score']:.3f}")
    
    # Print feature importance
    feature_importance = pd.DataFrame({
        'feature': X.columns,
        'importance': model.feature_importances_
    }).sort_values('importance', ascending=False)
    
    print("\nFeature Importance:")
    print(feature_importance)
    
    # Save the model and label encoders
    print("\nSaving model and encoders...")
    joblib.dump(model, 'eta_predictor_model.joblib')
    joblib.dump(label_encoders, 'label_encoders.joblib')
    print("Model and encoders saved successfully!")

if __name__ == "__main__":
    main()
```

`load_and_preprocess_data` reads the `delivery_data.jsonl` file line by line, loads the data into a pandas DataFrame, converts timestamp strings into proper datetime objects, and extracts basic time features like the hour of the day and day of the week.

`load_and_preprocess_data` and `prepare_features_and_target` create the inputs for our model. They calculate the direct distance (using `geopy.distance.geodesic`) between the driver's current location and the destination for each data point, calculate the difference (in minutes) between the `initial_planned_eta` and the current `timestamp`, and convert non-numeric features like `status`, `traffic_level`, and `weather` into numbers using `sklearn.preprocessing.LabelEncoder`. We need to save these encoders so the predictor can apply the *exact same* transformation later.

What are we actually trying to predict? We want the model to predict the `actual_remaining_minutes` until the delivery is completed. This is calculated in `prepare_features_and_target` by looking at the *final* timestamp recorded for each unique `driver_id` in the training data and finding the difference between that final time and the current timestamp for each row. This gives us the “ground truth” for training.

`train_and_evaluate_model` splits the data into training and testing sets. A `RandomForestRegressor` from scikit-learn is used. A Random Forest is a good choice as they are robust, handle non-linear relationships well, and can provide feature importance insights. Key hyperparameters like `n_estimators` (number of trees) and `max_depth` are also set here.

The model is trained using the `.fit()` method on the training data (`X_train`, `y_train`). Then predictions are made on the unseen test set (`X_test`). After that, performance is evaluated using standard regression metrics: Mean Absolute Error (MAE), Root Mean Squared Error (RMSE), and R² Score. These tell us how accurate the predictions are on average and how much variance the model explains. Feature importances are also extracted from the trained model to see which factors the model found most predictive at this point.

Finally, the trained `model` and the `label_encoders` dictionary are saved to disk using `joblib.dump`. This allows the separate `predictor.py` script to load and use them without retraining. The idea is to train the model once on a large dataset. Then reuse it over and over again on real-time data as it comes in.

This script transforms the raw simulated data into a predictive model ready for real-time application.

### Real-Time Predictor

We have a simulator generating data and a model trained on that data. Now we need the component that actually *uses* the trained model in real-time. This is `predictor.py`.

It monitors the stream of incoming delivery data (from `delivery_data.jsonl`), loads the pre-trained machine learning model and encoders, processes each new data point, predicts the updated ETA using the model, and displays the results. This script simulates a live prediction service.

```python
import json
import time
import pandas as pd
import joblib
from typing import Dict
from geopy.distance import geodesic
import os

class ETAPredictor:
    def __init__(self, model_path: str = 'eta_predictor_model.joblib', 
                 encoders_path: str = 'label_encoders.joblib'):
        """Initialize the ETA predictor with trained model and encoders."""
        print("Loading model and encoders...")
        self.model = joblib.load(model_path)
        self.label_encoders = joblib.load(encoders_path)
        self.last_processed_position = 0
        
    def prepare_features(self, data_point: Dict) -> pd.DataFrame:
        """Prepare features for a single data point."""
        # Convert timestamp and calculate time-based features
        timestamp = pd.to_datetime(data_point['timestamp'])
        initial_eta = pd.to_datetime(data_point['initial_planned_eta'])
        
        # Calculate distance remaining
        distance_remaining = geodesic(
            (data_point['current_latitude'], data_point['current_longitude']),
            (data_point['destination_latitude'], data_point['destination_longitude'])
        ).miles
        
        # Create feature dictionary
        features = {
            'distance_remaining_mi': distance_remaining,
            'current_speed_mph': data_point['current_speed_mph'],
            'hour_of_day': timestamp.hour,
            'day_of_week': timestamp.dayofweek,
            'status_encoded': self.label_encoders['status'].transform([data_point['status']])[0],
            'traffic_level_encoded': self.label_encoders['traffic_level'].transform([data_point['traffic_level']])[0],
            'weather_encoded': self.label_encoders['weather'].transform([data_point['weather']])[0],
            'time_until_planned_eta': (initial_eta - timestamp).total_seconds() / 60  # minutes
        }
        
        return pd.DataFrame([features])
    
    def predict_eta(self, data_point: Dict) -> Dict:
        """Generate ETA prediction for a single data point."""
        # Prepare features
        X = self.prepare_features(data_point)
        
        # Make prediction (remaining minutes)
        predicted_minutes = float(self.model.predict(X)[0])
        
        # Calculate new ETA
        current_time = pd.to_datetime(data_point['timestamp'])
        new_eta = current_time + pd.Timedelta(minutes=predicted_minutes)
        
        # Calculate the difference from initial ETA
        initial_eta = pd.to_datetime(data_point['initial_planned_eta'])
        eta_difference = (new_eta - initial_eta).total_seconds() / 60  # minutes
        
        return {
            'driver_id': data_point['driver_id'],
            'current_time': current_time.isoformat(),
            'initial_eta': initial_eta.isoformat(),
            'predicted_eta': new_eta.isoformat(),
            'predicted_remaining_minutes': predicted_minutes,
            'eta_difference_minutes': eta_difference,
            'current_status': data_point['status'],
            'current_speed_mph': data_point['current_speed_mph'],
            'distance_remaining_mi': geodesic(
                (data_point['current_latitude'], data_point['current_longitude']),
                (data_point['destination_latitude'], data_point['destination_longitude'])
            ).miles
        }

def monitor_delivery_data(predictor: ETAPredictor, data_file: str = 'delivery_data.jsonl'):
    """Monitor the delivery data file for new records and generate predictions."""
    print(f"Monitoring {data_file} for new delivery updates...")
    print("Press Ctrl+C to stop\n")
    
    try:
        while True:
            # Check if file exists
            if not os.path.exists(data_file):
                time.sleep(1)
                continue
            
            # Read new data points
            with open(data_file, 'r') as f:
                # Seek to last processed position
                f.seek(predictor.last_processed_position)
                
                # Process new lines
                for line in f:
                    data_point = json.loads(line)
                    prediction = predictor.predict_eta(data_point)
                    
                    # Format and print prediction
                    eta_diff = prediction['eta_difference_minutes']
                    eta_status = "ON TIME" if abs(eta_diff) < 5 else "DELAYED" if eta_diff > 0 else "EARLY"
                    
                    print(f"[{prediction['current_time']}] Driver {prediction['driver_id']} | "
                          f"Status: {prediction['current_status']:>13} | "
                          f"Distance: {prediction['distance_remaining_mi']:>6.2f} mi | "
                          f"Speed: {prediction['current_speed_mph']:>6.1f} mph")
                    print(f"    Initial ETA: {prediction['initial_eta']}")
                    print(f"    Updated ETA: {prediction['predicted_eta']} "
                          f"({abs(eta_diff):.1f} min {'later' if eta_diff > 0 else 'earlier'} | {eta_status})")
                    print("-" * 100)
                
                # Update last processed position
                predictor.last_processed_position = f.tell()
            
            # Sleep briefly before next check
            time.sleep(1)
            
    except KeyboardInterrupt:
        print("\nStopped monitoring delivery data.")

def main():
    predictor = ETAPredictor()
    monitor_delivery_data(predictor)

if __name__ == "__main__":
    main()
```

`ETAPredictor.__init__` starts loading everything. When the predictor starts, the `ETAPredictor` class immediately loads the essential files created by `train_model.py`.

`eta_predictor_model.joblib` is the trained Random Forest model.

`label_encoders.joblib` is the dictionary containing the fitted `LabelEncoder` objects for categorical features. This is crucial for ensuring consistency between training and prediction.

This `__init__` also initializes `last_processed_position` to 0. This variable will track how much of the `delivery_data.jsonl` file has already been processed.

`ETAPredictor.prepare_features` prepares the live data. It takes a single incoming data point (a Python dictionary parsed from a JSON line). Its job is to transform this raw data into the *exact* feature format the model expects. It calculates features like `distance_remaining_mi` (using `geopy`) and extracts time features (`hour_of_day`, `day_of_week`). It uses the *loaded* `label_encoders` to convert the string values for `status`, `traffic_level`, and `weather` into the same numerical codes used during training (e.g., transforming 'EN\_ROUTE' into its corresponding integer). Finally, it returns a single-row pandas DataFrame ready for the model.

Predictions are made with `ETAPredictor.predict_eta.` It takes the prepared feature DataFrame from `prepare_features` and calls the loaded model's `.predict()` method. The model outputs the predicted remaining delivery time in minutes.

It calculates the new ETA by adding the predicted remaining minutes (as a `timedelta`) to the timestamp of the current data point, calculates the difference between this new predicted ETA and the `initial_planned_eta` to see if the delivery is early, late, or on time, and returns a dictionary containing the prediction results and relevant context.

`monitor_delivery_data` acts like a real-time listener. It runs in an infinite loop. Inside the loop, it opens the `delivery_data.jsonl` file and uses `f.seek(predictor.last_processed_position)` to jump directly to the end of the previously read data. This prevents reprocessing old data. It then reads any *new* lines that have been added to the file since the last check.

For each new line (parsed as `data_point`), it calls `predictor.predict_eta()`. Then it formats the prediction results into a user-friendly string, showing the driver ID, current status, distance, speed, initial ETA, and the updated predicted ETA with the difference highlighted (e.g., "15.2 min later | DELAYED").

After reading all new lines, it updates `predictor.last_processed_position = f.tell()` to remember where it stopped reading for the next iteration, and it sleeps for a short interval (`time.sleep(1)`) before checking the file again.

This script brings everything together, applying the trained intelligence to the simulated live data feed.

## Getting Started: Run the System Yourself

Now that we've seen the components, let's walk through how you can run this system yourself.

### Setup

1. **Get the Code:** First, you'll need the code. You can clone the repository from [https://github.com/travishorn/delivery\_eta\_prediction](https://github.com/travishorn/delivery_eta_prediction).
    
    ```python
    git clone https://github.com/travishorn/delivery_eta_prediction
    cd delivery_eta_prediction
    ```
    
2. **Create a Virtual Environment:** It's best practice to use a Python virtual environment to manage dependencies.
    
    * On macOS/Linux:
        
        ```python
        python3 -m venv venv
        source venv/bin/activate
        ```
        
    * On Windows:
        
        ```python
        python -m venv venv
        .\venv\Scripts\activate
        ```
        
3. **Install Dependencies:** The required libraries are listed in `requirements.txt`. Install them using pip:
    
    ```python
    pip install -r requirements.txt
    ```
    

### Execution Flow

**Step 1: Generate Training Data**

Before you can train the model, you need some data. Run the simulator to generate it:

```bash
python src/simulator.py
```

**Tip:** To generate data quickly, you can edit `simulator.py` and set a high `time_acceleration_factor` in the `main` block (e.g., `time_acceleration_factor=60.0` or higher). This makes simulated time pass much faster than real-time.

This script will output data points to `delivery_data.jsonl` and print status updates to the console. Let it run until all simulated deliveries are complete. You might want to run it a few times or increase `num_drivers` to generate a larger dataset for potentially better model performance.

**Step 2: Train the ML Model**

Once you have `delivery_data.jsonl`, train the model:

```bash
python src/train_model.py
```

This script loads `delivery_data.jsonl`, processes it, trains the Random Forest model, and evaluates it. It saves the trained model as `eta_predictor_model.joblib` and the necessary label encoders as `label_encoders.joblib`.

**Step 3: Run the Real-Time System**

Now for the fun part! You'll need two separate terminal windows (make sure your virtual environment is activated in both).

* **Terminal 1 - Start the Simulator:**
    
    ```bash
    python src/simulator.py
    ```
    
    **Tip:** For this step, you'll likely want to set `time_acceleration_factor=1.0` in `simulator.py` so it runs in (simulated) real-time, mimicking a live data feed.
    
* **Terminal 2 - Start the Prediction Service:**
    
    ```bash
    python src/predictor.py
    ```
    

Now, as the simulator (Terminal 1) generates new data points and writes them to `delivery_data.jsonl`, the predictor (Terminal 2) will automatically pick them up, make predictions, and display the updated ETAs. You should see output similar to this in Terminal 2:

```bash
[2025-04-13T19:07:00] Driver D1 | Status:     EN_ROUTE | Distance:  15.34 mi | Speed:  42.1 mph
    Initial ETA: 2025-04-13T19:35:00
    Updated ETA: 2025-04-13T19:38:12 (3.2 min later | ON TIME)
----------------------------------------------------------------------------------------------------
[2025-04-13T19:07:10] Driver D2 | Status: DELAYED_TRAFFIC | Distance:   8.91 mi | Speed:  15.5 mph
    Initial ETA: 2025-04-13T19:25:00
    Updated ETA: 2025-04-13T19:45:30 (20.5 min later | DELAYED)
----------------------------------------------------------------------------------------------------
```

And that's it! You now have the simulator feeding data to the predictor, which uses the trained ML model to generate real-time ETA updates.

## Things to Keep in Mind

The current simulator uses straight-line distance calculations and linear interpolation for movement. Real-world routes follow road networks, and travel times are heavily influenced by speed limits, turns, and actual road conditions. The random events (traffic, stops) are also quite basic compared to the complexity of real traffic patterns and delay causes.

Real-world data might include richer information (e.g., specific road segments, vehicle type, detailed package information) that could improve predictions. The target variable (time to the final timestamp of the trip) is also a simplification; a more robust target might involve predicting time to specific future waypoints.

While the features used (distance, speed, time, status, weather, traffic) are relevant, more sophisticated features could be engineered with real-world data. For example, representing location more effectively than just raw latitude/longitude or capturing the progress along a planned route might yield better results.

The current predictor monitors a simple file (`delivery_data.jsonl`). A production system would typically use a more robust and scalable real-time data pipeline, likely involving message queues (like Kafka or RabbitMQ) or streaming platforms to handle potentially high volumes of incoming data from many drivers.

## Enhancements

Here are some ideas that could transform this proof-of-concept into a much more powerful and realistic tool:

* Replace the linear interpolation in the simulator with a real routing engine like OSRM or GraphHopper
    
* Incorporate live traffic data from services like the Google Maps Platform to make the simulation and predictions even more accurate.
    
* Experiment with different machine learning models known for handling tabular or sequential data well. Gradient boosting or recurrent neural networks could be useful here.
    
* Explore features like time spent at recent stops, deviation from the planned route, historical traffic density for the current road segment/time, or driver-specific behavior patterns.
    
* Use a message queue system to decouple the simulator and predictor. There’s a lot of room for scalability improvements here.
    
* Containerize and deploy the code to a cloud platform.
    
* Create a web-based dashboard to visualize driver locations on a map in real-time, display predicted ETAs, and show system status.
    

## Conclusion

You can see how the three core components work together: the `simulator.py` script generates a stream of realistic delivery data, `train_model.py` learns patterns from this data to build a predictive model, and `predictor.py` applies this model to live data for dynamic ETA updates.

While this project uses simulated data and has room for improvement, it demonstrates the core ideas of applying machine learning to logistics challenges. Being able to dynamically adjust ETAs based on real-time conditions is a powerful thing.

I encourage you to check out the code repository and try running it yourself.

%[https://github.com/travishorn/delivery_eta_prediction] 

Feel free to play around with the code, experiment with different parameters, or try implementing some of the enhancement ideas. I hope this post gave you some insight into building a real-time prediction system.