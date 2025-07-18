# WORKFORCE-WELLNESS-ANALYTICS
import tkinter as tk
from tkinter import messagebox
import pandas as pd
import matplotlib.pyplot as plt
import datetime

# Global DataFrame to store all entries
wellness_data = pd.DataFrame(columns=[
    "Date", "Sleep Hours", "Screen Time", "Water Intake", "Activity (mins)",
    "Aptitude Study (hrs)", "Programming Study (hrs)", "Mood", "Productivity"
])

# Save entry to DataFrame
def save_entry():
    try:
        date = datetime.date.today()
        sleep = float(sleep_entry.get())
        screen = float(screen_entry.get())
        water = float(water_entry.get())
        activity = float(activity_entry.get())
        aptitude = float(aptitude_entry.get())
        programming = float(programming_entry.get())
        mood = mood_var.get()
        productivity = int(productivity_entry.get())

        global wellness_data
        wellness_data = pd.concat([
            wellness_data,
            pd.DataFrame([[date, sleep, screen, water, activity,
                           aptitude, programming, mood, productivity]],
                         columns=wellness_data.columns)
        ], ignore_index=True)

        messagebox.showinfo("Success", "Wellness data saved!")
        clear_fields()

    except ValueError:
        messagebox.showerror("Input Error", "Please enter valid numeric values.")

# Clear all inputs
def clear_fields():
    sleep_entry.delete(0, tk.END)
    screen_entry.delete(0, tk.END)
    water_entry.delete(0, tk.END)
    activity_entry.delete(0, tk.END)
    aptitude_entry.delete(0, tk.END)
    programming_entry.delete(0, tk.END)
    productivity_entry.delete(0, tk.END)
    mood_var.set("Happy")

# Show analytics
def show_dashboard():
    if wellness_data.empty:
        messagebox.showwarning("No Data", "Add some data first!")
        return

    wellness_data["Date"] = pd.to_datetime(wellness_data["Date"])

    plt.figure(figsize=(12, 7))
    plt.suptitle("Student Wellness Dashboard", fontsize=16)

    plt.subplot(2, 2, 1)
    plt.plot(wellness_data["Date"], wellness_data["Sleep Hours"], label="Sleep", marker='o')
    plt.plot(wellness_data["Date"], wellness_data["Screen Time"], label="Screen Time", marker='s')
    plt.legend()
    plt.title("Sleep vs Screen Time")

    plt.subplot(2, 2, 2)
    plt.plot(wellness_data["Date"], wellness_data["Aptitude Study (hrs)"], label="Aptitude", marker='^')
    plt.plot(wellness_data["Date"], wellness_data["Programming Study (hrs)"], label="Programming", marker='v')
    plt.legend()
    plt.title("Study Time Tracking")

    plt.subplot(2, 2, 3)
    plt.plot(wellness_data["Date"], wellness_data["Water Intake"], color='blue', label="Water (L)", marker='o')
    plt.plot(wellness_data["Date"], wellness_data["Activity (mins)"], color='green', label="Activity", marker='x')
    plt.legend()
    plt.title("Hydration & Activity")

    plt.subplot(2, 2, 4)
    plt.plot(wellness_data["Date"], wellness_data["Productivity"], color='purple', marker='*')
    plt.title("Productivity Level")

    plt.tight_layout(rect=[0, 0.03, 1, 0.95])
    plt.show()

# GUI Setup
root = tk.Tk()
root.title("💼 Workforce Wellness Dashboard for Students")
root.geometry("500x600")
root.config(bg="#f0f4f8")

# Input Labels and Entries
def create_label_entry(label_text, row):
    label = tk.Label(root, text=label_text, bg="#f0f4f8", font=("Arial", 10, "bold"))
    label.grid(row=row, column=0, sticky="w", padx=15, pady=5)
    entry = tk.Entry(root, width=25)
    entry.grid(row=row, column=1, padx=10, pady=5)
    return entry

sleep_entry = create_label_entry("Sleep Hours:", 0)
screen_entry = create_label_entry("Screen Time (hrs):", 1)
water_entry = create_label_entry("Water Intake (L):", 2)
activity_entry = create_label_entry("Physical Activity (mins):", 3)
aptitude_entry = create_label_entry("Aptitude Study (hrs):", 4)
programming_entry = create_label_entry("Programming Study (hrs):", 5)
productivity_entry = create_label_entry("Productivity (0-10):", 6)

# Mood dropdown
tk.Label(root, text="Mood:", bg="#f0f4f8", font=("Arial", 10, "bold")).grid(row=7, column=0, sticky="w", padx=15, pady=5)
mood_var = tk.StringVar()
mood_var.set("Happy")
mood_options = ["Happy", "Neutral", "Sad", "Stressed"]
tk.OptionMenu(root, mood_var, *mood_options).grid(row=7, column=1, padx=10, pady=5)

# Buttons
tk.Button(root, text="Save Entry", command=save_entry, bg="#00aaff", fg="white", font=("Arial", 10, "bold")).grid(row=8, column=0, pady=20, padx=15)
tk.Button(root, text="View Dashboard", command=show_dashboard, bg="#28a745", fg="white", font=("Arial", 10, "bold")).grid(row=8, column=1)

root.mainloop()
