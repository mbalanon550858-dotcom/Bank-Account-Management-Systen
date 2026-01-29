class BankAccount:
    
    def __init__(self, account_number, account_holder, initial_balance=0.0):
     
        if initial_balance < 0:
            raise ValueError("Initial balance cannot be negative")
        
        self.account_number = account_number
        self.account_holder = account_holder
        self._balance = initial_balance 
        self.transaction_history = []
        
        if initial_balance > 0:
            self.transaction_history.append(f"Initial deposit: ${initial_balance:.2f}")
    
    def deposit(self, amount):
        
        if amount <= 0:
            raise ValueError("Deposit amount must be positive")
        
        self._balance += amount
        self.transaction_history.append(f"Deposit: +${amount:.2f}")
        return f"Successfully deposited ${amount:.2f}"
    
    def withdraw(self, amount):
       
        if amount <= 0:
            raise ValueError("Withdrawal amount must be positive")
        
        if amount > self._balance:
            raise RuntimeError(f"Insufficient funds. Available: ${self._balance:.2f}")
        
        self._balance -= amount
        self.transaction_history.append(f"Withdrawal: -${amount:.2f}")
        return f"Successfully withdrew ${amount:.2f}"
    
    def display_balance(self):
        return f"Account Balance: ${self._balance:.2f}"
    
    def display_transactions(self):
        if not self.transaction_history:
            return "No transactions yet."
        
        transactions = "\n".join(self.transaction_history)
        return f"Transaction History:\n{transactions}\nCurrent Balance: ${self._balance:.2f}"
    
    def get_balance(self):
        return self._balance


class BankSystem:

    def __init__(self):
        self.accounts = {}
    
    def create_account(self):
        print("\n" + "="*40)
        print("CREATE NEW ACCOUNT")
        print("="*40)
        
        try:
            account_number = input("Enter account number: ").strip()
            if not account_number:
                print("Error: Account number cannot be empty.")
                return
            
            if account_number in self.accounts:
                print(f"Error: Account number '{account_number}' already exists.")
                return
            
            account_holder = input("Enter account holder name: ").strip()
            if not account_holder:
                print("Error: Account holder name cannot be empty.")
                return
            
            initial_balance_str = input("Enter initial balance (default: 0): ").strip()
            if not initial_balance_str:
                initial_balance = 0.0
            else:
                try:
                    initial_balance = float(initial_balance_str)
                    if initial_balance < 0:
                        print("Error: Initial balance cannot be negative.")
                        return
                except ValueError:
                    print("Error: Please enter a valid number for balance.")
                    return
            
            self.accounts[account_number] = BankAccount(
                account_number, account_holder, initial_balance
            )
            
            print(f"\n✅ Account created successfully!")
            print(f"   Account Number: {account_number}")
            print(f"   Account Holder: {account_holder}")
            print(f"   Initial Balance: ${initial_balance:.2f}")
            
        except Exception as e:
            print(f"Unexpected error creating account: {e}")
    
    def perform_deposit(self):
        print("\n" + "="*40)
        print("DEPOSIT MONEY")
        print("="*40)
        
        try:
            account_number = input("Enter account number: ").strip()
            if account_number not in self.accounts:
                print(f"Error: Account '{account_number}' not found.")
                return
            
            amount_str = input("Enter amount to deposit: ").strip()
            try:
                amount = float(amount_str)
                result = self.accounts[account_number].deposit(amount)
                print(f"\n✅ {result}")
                print(f"   New balance: ${self.accounts[account_number].get_balance():.2f}")
            except ValueError as e:
                print(f"Error: {e}")
            except Exception as e:
                print(f"Error during deposit: {e}")
                
        except Exception as e:
            print(f"Unexpected error: {e}")
    
    def perform_withdrawal(self):
        print("\n" + "="*40)
        print("WITHDRAW MONEY")
        print("="*40)
        
        try:
            account_number = input("Enter account number: ").strip()
            if account_number not in self.accounts:
                print(f"Error: Account '{account_number}' not found.")
                return
            
            amount_str = input("Enter amount to withdraw: ").strip()
            try:
                amount = float(amount_str)
                result = self.accounts[account_number].withdraw(amount)
                print(f"\n✅ {result}")
                print(f"   New balance: ${self.accounts[account_number].get_balance():.2f}")
            except ValueError as e:
                print(f"Error: {e}")
            except RuntimeError as e:
                print(f"Error: {e}")
            except Exception as e:
                print(f"Error during withdrawal: {e}")
                
        except Exception as e:
            print(f"Unexpected error: {e}")
    
    def check_balance(self):
        """Check account balance."""
        print("\n" + "="*40)
        print("CHECK BALANCE")
        print("="*40)
        
        try:
            account_number = input("Enter account number: ").strip()
            if account_number not in self.accounts:
                print(f"Error: Account '{account_number}' not found.")
                return
            
            account = self.accounts[account_number]
            print(f"\nAccount Number: {account.account_number}")
            print(f"Account Holder: {account.account_holder}")
            print(account.display_balance())
            
        except Exception as e:
            print(f"Unexpected error: {e}")
    
    def view_transactions(self):
        """View transaction history."""
        print("\n" + "="*40)
        print("VIEW TRANSACTIONS")
        print("="*40)
        
        try:
            account_number = input("Enter account number: ").strip()
            if account_number not in self.accounts:
                print(f"Error: Account '{account_number}' not found.")
                return
            
            print(f"\nAccount: {self.accounts[account_number].account_holder}")
            print(self.accounts[account_number].display_transactions())
            
        except Exception as e:
            print(f"Unexpected error: {e}")
    
    def list_all_accounts(self):
        """List all accounts in the system."""
        print("\n" + "="*40)
        print("ALL ACCOUNTS")
        print("="*40)
        
        if not self.accounts:
            print("No accounts found.")
            return
        
        for i, (acc_num, account) in enumerate(self.accounts.items(), 1):
            print(f"\n{i}. Account Number: {acc_num}")
            print(f"   Holder: {account.account_holder}")
            print(f"   Balance: ${account.get_balance():.2f}")
    
    def run(self):
        """Run the main bank system menu."""
        print("\n" + "="*50)
        print("BANK ACCOUNT MANAGEMENT SYSTEM")
        print("="*50)
        
        while True:
            print("\n" + "-"*30)
            print("MAIN MENU")
            print("-"*30)
            print("1. Create New Account")
            print("2. Deposit Money")
            print("3. Withdraw Money")
            print("4. Check Balance")
            print("5. View Transaction History")
            print("6. List All Accounts")
            print("7. Exit")
            print("-"*30)
            
            choice = input("Enter your choice (1-7): ").strip()
            
            try:
                if choice == "1":
                    self.create_account()
                elif choice == "2":
                    self.perform_deposit()
                elif choice == "3":
                    self.perform_withdrawal()
                elif choice == "4":
                    self.check_balance()
                elif choice == "5":
                    self.view_transactions()
                elif choice == "6":
                    self.list_all_accounts()
                elif choice == "7":
                    print("\n" + "="*50)
                    print("Thank you for using our Bank System!")
                    print("="*50)
                    break
                else:
                    print("Invalid choice. Please enter a number between 1 and 7.")
            
            except KeyboardInterrupt:
                print("\n\nProgram interrupted. Exiting...")
                break
            except Exception as e:
                print(f"An unexpected error occurred: {e}")
                print("Please try again.")


def main():
    """Main function to run the bank system."""
    try:
        bank_system = BankSystem()
        bank_system.run()
    except Exception as e:
        print(f"A critical error occurred: {e}")
    finally:
        print("\nProgram terminated.")


if __name__ == "__main__":
    main()
