# code
codepyth
import json
import os

class Expense:
    def __init__(self, date, category, amount, description):
        self.date = date
        self.category = category
        self.amount = amount
        self.description = description

    def __str__(self):
        return f"{self.date} - {self.category}: {self.amount} ({self.description})"

class ExpenseTracker:
    def __init__(self, filename="expenses.json"):
        self.filename = filename
        self.expenses = []
        self.load_expenses()

    def load_expenses(self):
        if os.path.exists(self.filename):
            with open(self.filename, "r") as file:
                expenses_data = json.load(file)
                self.expenses = [Expense(**data) for data in expenses_data]
        else:
            self.expenses = []

    def save_expenses(self):
        with open(self.filename, "w") as file:
            json.dump([expense.__dict__ for expense in self.expenses], file, indent=4)

    def add_expense(self, date, category, amount, description):
        new_expense = Expense(date, category, amount, description)
        self.expenses.append(new_expense)
        self.save_expenses()

    def remove_expense(self, description):
        self.expenses = [expense for expense in self.expenses if expense.description != description]
        self.save_expenses()

    def edit_expense(self, old_description, new_date, new_category, new_amount, new_description):
        for expense in self.expenses:
            if expense.description == old_description:
                expense.date = new_date
                expense.category = new_category
                expense.amount = new_amount
                expense.description = new_description
                self.save_expenses()
                break

    def display_expenses(self):
        if not self.expenses:
            print("No expenses found.")
        else:
            for expense in self.expenses:
                print(expense)
                print("-" * 30)

def main():
    tracker = ExpenseTracker()

    while True:
        print("\nExpense Tracker Menu")
        print("1. Add expense")
        print("2. Remove expense")
        print("3. Edit expense")
        print("4. View expenses")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            date = input("Enter expense date (YYYY-MM-DD): ")
            category = input("Enter expense category: ")
            amount = float(input("Enter expense amount: "))
            description = input("Enter expense description: ")
            tracker.add_expense(date, category, amount, description)
        elif choice == "2":
            description = input("Enter the description of the expense to remove: ")
            tracker.remove_expense(description)
        elif choice == "3":
            old_description = input("Enter the description of the expense to edit: ")
            new_date = input("Enter new expense date (YYYY-MM-DD): ")
            new_category = input("Enter new expense category: ")
            new_amount = float(input("Enter new expense amount: "))
            new_description = input("Enter new expense description: ")
            tracker.edit_expense(old_description, new_date, new_category, new_amount, new_description)
        elif choice == "4":
            tracker.display_expenses()
        elif choice == "5":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
