import pandas as pd
import matplotlib.pyplot as plt

# Sample time series data: Daily temperature (in °C)
data = {
    'Date': pd.date_range(start='2023-01-01', periods=30, freq='D'),
    'Temperature': [20 + (x % 10) for x in range(30)]  # Sample temperature data
}

# Create a DataFrame
df = pd.DataFrame(data)
df.set_index('Date', inplace=True)

# Function to plot the time series
def plot_time_series(dataframe):
    plt.figure(figsize=(10, 5))
    plt.plot(dataframe.index, dataframe['Temperature'], marker='o', linestyle='-', color='b')
    plt.title('Daily Temperature Over Time')
    plt.xlabel('Date')
    plt.ylabel('Temperature (°C)')
    plt.grid()
    plt.xticks(rotation=45)
    
    # Annotating trends or patterns
    plt.annotate('Warm Period', xy=(pd.Timestamp('2023-01-10'), 25), 
                 xytext=(pd.Timestamp('2023-01-15'), 27),
                 arrowprops=dict(facecolor='black', shrink=0.05))
    
    plt.tight_layout()
    plt.show()

# Function to update or add temperature values
def update_temperature(date, new_value):
    try:
        date_dt = pd.to_datetime(date)
        if date_dt in df.index:
            df.at[date_dt, 'Temperature'] = new_value
            print(f"Updated temperature on {date} to {new_value}°C")
        else:
            # If date doesn't exist, add a new entry
            df.loc[date_dt] = new_value
            print(f"Added new temperature entry on {date} with value {new_value}°C")
            df.sort_index(inplace=True)  # Sort by date after adding
    except Exception as e:
        print(f"Error: {e}")

# Initial plot
plot_time_series(df)

# User input for updating or adding values
while True:
    user_input = input("Do you want to update or add a temperature value? (yes/no): ").strip().lower()
    if user_input == 'yes':
        date_input = input("Enter the date (YYYY-MM-DD): ")
        new_value_input = float(input("Enter the new temperature value (°C): "))
        update_temperature(date_input, new_value_input)
        plot_time_series(df)  # Re-plot after update or addition
    elif user_input == 'no':
        break
    else:
        print("Please enter 'yes' or 'no'.")
