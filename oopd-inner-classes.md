# oopd Inner classes

**ATM the distinction can not be articulated!**

These four distinguishable inner classes will be talked about in this document:
* static
* instance
* local
* anonymous

There might be more inner classes but those are not known.

## static inner class
used to access superclass attributes and where only one instance of the child should be made. Like for child static attributes.
Example, examine the static Permissions class, which is an inner class of Account_3_20:
``` Java
public class Account_3_20 {
	private int userId;
	private Permissions perm;

	public Account_3_20(int userId) {
		this.userId = userId;
		perm = new Permissions();
	}

	public int getUserId() {
		return userId;
	}

	public static class Permissions {
		public boolean canRead;
		public boolean canWrite;
		public boolean canDelete;
	}

	public Permissions getPermissions() {
		return perm;
	}
}

class Test {
	public static void main(String[] args) {
		Account_3_20 account = new Account_3_20(4711);

		Account_3_20.Permissions perm = account.getPermissions();
		perm.canRead = true;

		System.out.println(perm.canRead);
		System.out.println(perm.canWrite);
		System.out.println(perm.canDelete);
	}
}
```

## instance inner class
This might be good in RPG, to display hit damage for example, look at health points as the balance from the account, the deposit, as drinking health potions, the withdraw as damage, and the system out or toString as the display for the user to get info on what is going on.
Note the Instance created with deposit will be lost after the creation of a new Instance with withdraw.
Example:
``` Java
public class Account_3_21 {
	private int accountNumber;
	private double balance;
	private Transaction last;

	public Account_3_21(int accountNumber, double balance) {
		super();
		this.accountNumber = accountNumber;
		this.balance = balance;
	}

	public class Transaction {
		private String name;
		private double amount;

		public Transaction(String name, double amount) {
			this.name = name;
			this.amount = amount;
		}

		public String toString() {
			return accountNumber + ": " + name + " " + amount + ", balance "
					+ balance;
		}
	}

	public Transaction getLast() {
		return last;
	}

	public void deposit(double amount) {
		balance += amount;
		last = new Transaction("deposit", amount);
	}

	public void withdraw(double amount) {
		balance -= amount;
		last = new Transaction("withdraw", amount);
	}
}

class TestAccount_3_21 {
	public static void main(String[] args) {
		Account_3_21 a = new Account_3_21(4771, 1000.);

		a.deposit(500.);
		a.withdraw(700.);

		Account_3_21.Transaction t = a.getLast();
		System.out.println(t.toString());
	}
}
```

## local inner class
The local class can be used inside a method, constructor or initializing block like local variables.
It can access all the methods and variables of the surrounding class as long as it is not inside a class method or static block.
``` Java

```

## anonymous inner class
Like local inner class, but generating the class without a name. See: https://docs.oracle.com/javase/tutorial/java/javaOO/anonymousclasses.html
``` Java

```
