# python_project
my money expenditure
import os

FILENAME = "expenses.txt"


def load_expenses():
  """Load existing expenses from the file into a list of dictionaries."""
  expenses = []
  if os.path.exists(FILENAME):
    with open(FILENAME, "r") as file:
      for line in file:
        category, amount, desc = line.strip().split("|")
        expenses.append(
            {"category": category, "amount": float(amount), "desc": desc}
        )
  return expenses


def save_expense(category, amount, desc):
  """Save a single new expense to the file."""
  with open(FILENAME, "a") as file:
    file.write(f"{category}|{amount}|{desc}\n")


def add_expense(expenses):
  """Add a new expense entry."""
  print("\n--- ADD NEW EXPENSE ---")
  category = input("Enter Category (e.g., Food, Travel, Rent): ").strip()

  try:
    amount = float(input("Enter Amount: ₹"))
  except ValueError:
    print("Invalid amount! Please enter a number.\n")
    return

  desc = input("Enter Short Description (optional): ").strip()
  if not desc:
    desc = "N/A"

  # Save locally and append to file
  expenses.append({"category": category, "amount": amount, "desc": desc})
  save_expense(category, amount, desc)
  print(f"Added ₹{amount:.2f} under '{category}' successfully!\n")


def view_summary(expenses):
  """Show total spent and breakdown by category."""
  if not expenses:
    print("\nNo expenses recorded yet.\n")
    return

  print("\n================ EXPENSE SUMMARY ================")
  total = 0
  category_totals = {}

  for item in expenses:
    cat = item["category"]
    amt = item["amount"]
    total += amt
    category_totals[cat] = category_totals.get(cat, 0) + amt

  # Print by category
  print(f"{'Category':<20} | {'Total Spent':<10}")
  print("-" * 35)
  for cat, cat_amt in category_totals.items():
    print(f"{cat:<20} | ₹{cat_amt:<10.2f}")

  print("=" * 35)
  print(f"GRAND TOTAL: ₹{total:.2f}")
  print("=================================================\n")


def view_all(expenses):
  """Show all individual transactions."""
  if not expenses:
    print("\nNo expenses recorded yet.\n")
    return

  print("\n================ ALL TRANSACTIONS ================")
  print(f"{'#':<4} | {'Category':<15} | {'Amount':<10} | {'Description'}")
  print("-" * 55)

  for idx, item in enumerate(expenses, 1):
    print(
        f"{idx:<4} | {item['category']:<15} | ₹{item['amount']:<9.2f} |"
        f" {item['desc']}"
    )

  print("==================================================\n")


def main():
  expenses = load_expenses()

  while True:
    print("===== MONEY EXPENSE TRACKER =====")
    print("1. Add Expense")
    print("2. View Category Summary")
    print("3. View All Transactions")
    print("4. Exit")

    choice = input("Select an option (1-4): ").strip()

    if choice == "1":
      add_expense(expenses)
    elif choice == "2":
      view_summary(expenses)
    elif choice == "3":
      view_all(expenses)
    elif choice == "4":
      print("\nThank you for using Expense Tracker. Goodbye!")
      break
    else:
      print("Invalid option! Please select 1, 2, 3, or 4.\n")


if __name__ == "__main__":
  main()
