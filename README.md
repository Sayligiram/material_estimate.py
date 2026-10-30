
import csv
from datetime import datetime
import os

FILE_NAME = "material_log.csv"


def add_material():
    materials = []
    total = 0

    n = int(input("Enter number of materials: "))

    for i in range(n):
        print("\nMaterial", i + 1)

        name = input("Enter material name: ")
        quantity = float(input("Enter quantity: "))
        rate = float(input("Enter rate per unit: "))

        amount = quantity * rate
        total += amount

        materials.append([
            name, quantity, rate, amount
        ])

        print("Material Cost: Rs.", amount)

    print("\n========== MATERIAL ESTIMATE ==========")

    for item in materials:
        print(
            item[0], "| Quantity:", item[1],
            "| Rate:", item[2],
            "| Amount: Rs.", item[3]
        )

    print("---------------------------------------")
    print("Grand Total: Rs.", total)

    save_log(materials, total)


def save_log(materials, total):
    file_exists = os.path.exists(FILE_NAME)
    date = datetime.now().strftime("%d-%m-%Y %H:%M:%S")

    with open(FILE_NAME, "a", newline="") as file:
        writer = csv.writer(file)

        if not file_exists:
            writer.writerow([
                "Date", "Material", "Quantity",
                "Rate", "Amount", "Grand Total"
            ])

        for item in materials:
            writer.writerow([
                date, item[0], item[1],
                item[2], item[3], total
            ])

    print("\nEstimate and log saved successfully!")


def view_log():
    try:
        with open(FILE_NAME, "r") as file:
            reader = csv.reader(file)

            print("\n========== MATERIAL LOG ==========")

            for row in reader:
                print(" | ".join(row))

    except FileNotFoundError:
        print("No log found. Generate an estimate first.")


def main():
    while True:
        print("\n===== MATERIAL ESTIMATE SYSTEM =====")
        print("1. Generate Material Estimate")
        print("2. View Material Log")
        print("3. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            try:
                add_material()
            except ValueError:
                print("Please enter valid numeric values.")

        elif choice == "2":
            view_log()

        elif choice == "3":
            print("Thank you for using the system!")
            break

        else:
            print("Invalid choice. Please try again.")


main()
# material_estimate.py
A Python-based project that calculates material costs automatically, generates estimates, and stores material records in a CSV log file.
