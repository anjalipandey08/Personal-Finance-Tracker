from datetime import datetime

# Node for Linked List 
class Transaction:
    def __init__(self, t_type, amount, category, note):
        self.t_type = t_type
        self.amount = amount
        self.category = category
        self.note = note
        self.time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
        self.next = None


# Finance Tracker 
class FinanceTracker:
    def __init__(self):
        self.head = None
        self.balance = 0
        self.undo_stack = []

    # Add transaction
    def add_transaction(self, t_type, amount, category, note):
        new_txn = Transaction(t_type, amount, category, note)

        if not self.head:
            self.head = new_txn
        else:
            temp = self.head
            while temp.next:
                temp = temp.next
            temp.next = new_txn

        # Update balance
        if t_type == "Income":
            self.balance += amount
        else:
            self.balance -= amount

        self.undo_stack.append(new_txn)
        print("Transaction added successfully.")

    # Show transactions
    def show_transactions(self):
        if not self.head:
            print("No transactions found.")
            return

        temp = self.head
        print("\nTransaction History:")
        while temp:
            print(f"[{temp.time}] {temp.t_type} | ₹{temp.amount} | "
                  f"{temp.category} | {temp.note}")
            temp = temp.next

    # Undo last transaction
    def undo(self):
        if not self.undo_stack:
            print("Nothing to undo.")
            return

        last = self.undo_stack.pop()

        # Remove from linked list
        if self.head == last:
            self.head = None
        else:
            temp = self.head
            while temp.next != last:
                temp = temp.next
            temp.next = None

        # Reverse balance
        if last.t_type == "Income":
            self.balance -= last.amount
        else:
            self.balance += last.amount

        print("Last transaction undone.")

    def show_balance(self):
        print(f"Current Balance: ₹{self.balance}")


# Menu
def main():
    tracker = FinanceTracker()

    while True:
        print("\n====== Personal Finance Tracker ======")
        print("1. Add Income")
        print("2. Add Expense")
        print("3. Show Transactions")
        print("4. Show Balance")
        print("5. Undo Last Transaction")
        print("6. Exit")

        choice = input("Choose option: ")

        if choice == "1":
            amount = float(input("Enter income amount: "))
            category = input("Category: ")
            note = input("Note: ")
            tracker.add_transaction("Income", amount, category, note)

        elif choice == "2":
            amount = float(input("Enter expense amount: "))
            category = input("Category: ")
            note = input("Note: ")
            tracker.add_transaction("Expense", amount, category, note)

        elif choice == "3":
            tracker.show_transactions()

        elif choice == "4":
            tracker.show_balance()

        elif choice == "5":
            tracker.undo()

        elif choice == "6":
            print("Exiting Finance Tracker...")
            break

        else:
            print("Invalid option.")


if __name__ == "__main__":
    main()
