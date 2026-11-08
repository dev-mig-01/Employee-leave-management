# Employee-leave-management
python project for leave management of employees .

from datetime import datetime

employees = {
    "E101": {
        "name": "Amit",
        "balances":{
            "Casual": 12,
            "Sick": 10,
            "Earned": 15,
        }
    }
}

def parse_and_validate_dates(from_str,to_str):

    try:
        from_date= datetime.strptime(from_str.strip(), "%Y-%m-%d").date()
        to_date= datetime.strptime(to_str.strip(), "%Y-%m-%d").date()
    except ValueError:
        return None, "Invalid date format, please use YYYY-MM-DD."
    if to_date < from_date:
        return None, "Invalid date . To_date cannont be less than from_date"

    delta_days = (to_date - from_date).days + 1
    return delta_days, None

def apply_leave(emp_id, leave_type, from_str, to_str):
    emp_id = emp_id.strip()
    leave_type = leave_type.strip()

    emp = employees[emp_id]
    if leave_type not in emp["balances"]:
        print(f"Error: Invalid leave type '{leave_type}'. Allowed: Casual, Sick, Earned.")
        return
    
    days, err = parse_and_validate_dates(from_str, to_str)
    if err:
        print(err)
        return
    
    current_balance = emp["balances"][leave_type]
    if days <= current_balance:
        emp["balances"][leave_type] -= days
        print(f"Leave approved: {days} days {leave_type} | Balance {leave_type}: {emp['balances'][leave_type]}")
    else:
        print(f"Leave rejected. Requested: {days} days | Available: {current_balance} days")
    
def view_balance(emp_id):
    emp_id = emp_id.strip()
    if emp_id not in employees:
        print(f"Error: Employee ID '{emp_id}' not found.")
        return
    
    balances = employees[emp_id]['balances']
    print(f"Casual: {balances['Casual']} | Sick: {balances['Sick']} | Earned: {balances['Earned']}")

def main():
    while True:
        print("\n=== Employee Leave Management ===")
        print("1: Apply Leave")
        print("2: View Balance")
        print("3: Exit")
        choice = input("Enter choice (1-3): ").strip()

        if choice == 1:
            emp_id = input("Enter Employee ID(eg: E101): ")
            leave_type = input("Enter Leave Type:(Casual/Sick/Earned): ")
            print("#Enter date in format: YYYY-MM-DD.")
            from_date = input("Enter From Date: ")
            to_date = input("Enter To Date: ")
            apply_leave(emp_id, leave_type, from_date, to_date)

        elif choice == 2:
            emp_id = input("Enter Employee ID(eg: E101): ")
            view_balance(emp_id)
        
        elif choice == 3:
            print("Exiting Application.")

        else:
            print("Invalid choice. Please select 1, 2, or 3.")

if __name__ == "__main__":
    main()
    

