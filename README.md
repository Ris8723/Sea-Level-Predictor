# Sea-Level-Predictor
to create the observation of sea level predictor
import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

def draw_plot():
    # Load the data
    df = pd.read_csv('epa-sea-level.csv')
    
    # Create scatter plot
    plt.figure(figsize=(12, 6))
    plt.scatter(df['Year'], df['CSIRO Adjusted Sea Level'], color='blue', label='Original Data')
    
    # Perform linear regression on the entire dataset
    regression_all = linregress(df['Year'], df['CSIRO Adjusted Sea Level'])
    slope_all = regression_all.slope
    intercept_all = regression_all.intercept
    
    # Generate x values for the full dataset regression line
    x_all = pd.Series(range(1880, 2051))
    y_all = slope_all * x_all + intercept_all
    
    # Plot the line of best fit for all data
    plt.plot(x_all, y_all, 'r', label='Best Fit Line (All Data)')
    
    # Perform linear regression on data from 2000 onwards
    df_recent = df[df['Year'] >= 2000]
    regression_recent = linregress(df_recent['Year'], df_recent['CSIRO Adjusted Sea Level'])
    slope_recent = regression_recent.slope
    intercept_recent = regression_recent.intercept
    
    # Generate x values for the recent data regression line
    x_recent = pd.Series(range(2000, 2051))
    y_recent = slope_recent * x_recent + intercept_recent
    
    # Plot the line of best fit for recent data
    plt.plot(x_recent, y_recent, 'green', label='Best Fit Line (Since 2000)')
    
    # Add labels, title, and legend
    plt.title('Rise in Sea Level')
    plt.xlabel('Year')
    plt.ylabel('Sea Level (inches)')
    plt.legend()
    
    # Save plot and return the figure
    plt.savefig('sea_level_plot.png')
    return plt.gcf()

# Main execution (optional for local testing)
if __name__ == "__main__":
    draw_plot()
