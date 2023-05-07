# Import necessary libraries
import tkinter as tk
import sqlite3
import hashlib

# Create main window
root = tk.Tk()
root.title("Consegna App")

# Create login window
login_window = tk.Toplevel(root)
login_window.title("Login")

# Create registration window
registration_window = tk.Toplevel(root)
registration_window.title("Registration")

# Create database connection
conn = sqlite3.connect('consegna.db')
c = conn.cursor()

# Create users table
c.execute('''CREATE TABLE IF NOT EXISTS users
             (phone_number TEXT PRIMARY KEY, name TEXT, address TEXT, password_hash TEXT)''')

# Define functions for login and registration
def login():
    phone_number = phone_number_entry.get()
    password = password_entry.get()
    password_hash = hashlib.sha256(password.encode('utf-8')).hexdigest()
    c.execute('SELECT * FROM users WHERE phone_number=? AND password_hash=?', (phone_number, password_hash))
    if c.fetchone() is not None:
        login_window.destroy()
        # Open main app window
        # ...
    else:
        login_error_label.config(text="Invalid phone number or password")

def register():
    phone_number = phone_number_entry_reg.get()
    name = name_entry_reg.get()
    address = address_entry_reg.get()
    password = password_entry_reg.get()
    password_hash = hashlib.sha256(password.encode('utf-8')).hexdigest()
    c.execute('INSERT INTO users VALUES (?, ?, ?, ?)', (phone_number, name, address, password_hash))
    conn.commit()
    registration_window.destroy()
    # Open main app window
    # ...

# Create widgets for login window
phone_number_label = tk.Label(login_window, text="Phone Number:")
phone_number_entry = tk.Entry(login_window)
password_label = tk.Label(login_window, text="Password:")
password_entry = tk.Entry(login_window, show="*")
login_button = tk.Button(login_window, text="Login", command=login)
login_error_label = tk.Label(login_window, fg="red")

# Place widgets in login window
phone_number_label.pack()
phone_number_entry.pack()
password_label.pack()
password_entry.pack()
login_button.pack()
login_error_label.pack()

# Create widgets for registration window
phone_number_label_reg = tk.Label(registration_window, text="Phone Number:")
phone_number_entry_reg = tk.Entry(registration_window)
name_label_reg = tk.Label(registration_window, text="Name:")
name_entry_reg = tk.Entry(registration_window)
address_label_reg = tk.Label(registration_window, text="Address:")
address_entry_reg = tk.Entry(registration_window)
password_label_reg = tk.Label(registration_window, text="Password:")
password_entry_reg = tk.Entry(registration_window, show="*")
register_button = tk.Button(registration_window, text="Register", command=register)

# Place widgets in registration window
phone_number_label_reg.pack()
phone_number_entry_reg.pack()
name_label_reg.pack()
name_entry_reg.pack()
address_label_reg.pack()
address_entry_reg.pack()
password_label_reg.pack()
password_entry_reg.pack()
register_button.pack()

# Start the application
root.mainloop()

