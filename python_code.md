import csv
import os
from datetime import datetime

class FinanceTracker:
    def __init__(self, filename="finance_data.csv"):
        self.filename = filename
        # If file doesn't exist, create it with headers
        if not os.path.exists(self.filename):
            with open(self.filename, mode="w", newline="") as file:
                writer = csv.writer(file)
                writer.writerow(["Date", "Type", "Category", "Amount", "Note"])

    def add_record(self, record_type, category, amount, note=""):
        date = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        with open(self.filename, mode="a", newline="") as file:
            writer = csv.writer(file)
            writer.writerow([date, record_type, category, amount, note])
        print(f"{record_type} of {amount} added under {category}.")

    def show_summary(self):
        income, expense = 0, 0
        with open(self.filename, mode="r") as file:
            reader = csv.DictReader(file)
            for row in reader:
                amount = float(row["Amount"])
                if row["Type"] == "Income":
                    income += amount
                else:
                    expense += amount
        balance = income - expense
        print("\nFinance Summary")
        print(f"Total Income : ₹{income}")
        print(f"Total Expense: ₹{expense}")
        print(f"Balance      : ₹{balance}\n")

    def show_records(self):
        if os.path.getsize(self.filename) == 0:
            print("No records available.")
            return
        print("\nTransaction Records")
        with open(self.filename, mode="r") as file:
            reader = csv.DictReader(file)
            for row in reader:
                print(f"{row['Date']} | {row['Type']} | {row['Category']} | ₹{row['Amount']} | {row['Note']}")

# -------- MENU BASED PROGRAM --------
if __name__ == "__main__":
    tracker = FinanceTracker()

    while True:
        print("\n====== Personal Finance Tracker ======")
        print("1. Add Income")
        print("2. Add Expense")
        print("3. Show Summary")
        print("4. Show All Records")
        print("5. Exit")
        
        choice = input("Choose option: ")

        if choice == "1":
            category = input("Enter income category (e.g., Salary, Freelance): ")
            amount = float(input("Enter income amount: "))
            note = input("Enter note (optional): ")
            tracker.add_record("Income", category, amount, note)

        elif choice == "2":
            category = input("Enter expense category (e.g., Food, Rent, Travel): ")
            amount = float(input("Enter expense amount: "))
            note = input("Enter note (optional): ")
            tracker.add_record("Expense", category, amount, note)

        elif choice == "3":
            tracker.show_summary()

        elif choice == "4":
            tracker.show_records()

        elif choice == "5":
            print("Exiting... Goodbye!")
            break

        else:
            print("⚠️ Invalid choice! Please try again.")
