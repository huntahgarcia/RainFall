import pandas as pd
import matplotlib.pyplot as plt

def get_data(file1_path, file2_path):
    

    rainfall_set1 = pd.read_csv(file1_path, header=None, names=["Data"])
    rainfall_set2 = pd.read_csv(file2_path, header=None, names=["Data"])

    # Split the data into city and rainfall
    rainfall_set1[['City', 'Rainfall']] = rainfall_set1['Data'].str.split(expand=True)
    rainfall_set2[['City', 'Rainfall']] = rainfall_set2['Data'].str.split(expand=True)

    # Convert rainfall values to numeric
    rainfall_set1["Rainfall"] = pd.to_numeric(rainfall_set1["Rainfall"])
    rainfall_set2["Rainfall"] = pd.to_numeric(rainfall_set2["Rainfall"])

    # Drop the original 'Data' column
    rainfall_set1.drop(columns=['Data'], inplace=True)
    rainfall_set2.drop(columns=['Data'], inplace=True)

    return rainfall_set1, rainfall_set2

def comparison(rainfall_set1, rainfall_set2):
    plt.figure(figsize=(10, 6))
    plt.plot(rainfall_set1["City"], rainfall_set1["Rainfall"], label="Set 1")
    plt.plot(rainfall_set2["City"], rainfall_set2["Rainfall"], label="Set 2")
    plt.xlabel("City")
    plt.ylabel("Rainfall (inches)")
    plt.title("Comparison of Rainfall Data")
    plt.legend()
    plt.xticks(rotation=90)
    plt.grid(True)


def separate_plots(rainfall_set1, rainfall_set2):
    for i, dataset in enumerate([rainfall_set1, rainfall_set2], start=1):
        plt.figure(figsize=(10, 6))
        plt.plot(dataset["City"], dataset["Rainfall"], label=f"Set {i}")
        max_idx = dataset["Rainfall"].idxmax()
        min_idx = dataset["Rainfall"].idxmin()
        plt.scatter(dataset["City"][max_idx], dataset["Rainfall"][max_idx], color='red', label="Max")
        plt.scatter(dataset["City"][min_idx], dataset["Rainfall"][min_idx], color='green', label="Min")
        plt.xlabel("City")
        plt.ylabel("Rainfall (inches)")
        plt.title(f"Rainfall Data Set {i}")
        plt.legend()
        plt.xticks(rotation=90)
        plt.grid(True)
       

def box_plot(rainfall_set1, rainfall_set2):

    # Boxplots
    plt.figure(figsize=(10, 6))
    plt.boxplot([rainfall_set1["Rainfall"], rainfall_set2["Rainfall"]], labels=["Set 1", "Set 2"])
    plt.xlabel("Dataset")
    plt.ylabel("Rainfall (inches)")
    plt.title("Boxplot of Rainfall Data")
    plt.grid(True)
   


def main():
    file1_path = "/Users/carmengarcia/Desktop/Hunter/CS87B/rainfallSet1.txt"
    file2_path = "/Users/carmengarcia/Desktop/Hunter/CS87B/rainfallSet2.txt"
    
    rainfall_set1, rainfall_set2 = get_data(file1_path, file2_path)
    
    comparison(rainfall_set1, rainfall_set2)
    separate_plots(rainfall_set1, rainfall_set2)
    box_plot(rainfall_set1, rainfall_set2)

if __name__ == "__main__":
    main()
