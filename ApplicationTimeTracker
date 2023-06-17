import time
import psutil
import csv
from collections import defaultdict
from datetime import datetime

def get_active_window_title():
    active_window_pid = None
    for process in psutil.process_iter(['pid', 'name', 'connections']):
        if "ESTABLISHED" in str(process.connections()):
            active_window_pid = process.pid
            break

    if active_window_pid:
        for process in psutil.process_iter(['pid', 'name']):
            if process.pid == active_window_pid:
                return process.name()
    return None

def track_time_usage(duration=3600, interval=5, csv_file="time_usage.csv"):
    usage_data = defaultdict(int)
    start_time = time.time()
    end_time = start_time + duration

    while time.time() < end_time:
        active_window_title = get_active_window_title()
        if active_window_title:
            usage_data[active_window_title] += interval
        time.sleep(interval)

    total_time = sum(usage_data.values())
    date_today = datetime.now().strftime("%Y-%m-%d")

    # Save data to CSV file
    with open(csv_file, "a", newline="") as csvfile:
        fieldnames = ["date", "app_name", "time_spent", "percentage"]
        writer = csv.DictWriter(csvfile, fieldnames=fieldnames)

        for app, app_time in usage_data.items():
            percentage = (app_time / total_time) * 100
            writer.writerow({
                "date": date_today,
                "app_name": app,
                "time_spent": app_time,
                "percentage": f"{percentage:.2f}"
            })

if __name__ == "__main__":
    print("Time Tracker started...")
    track_time_usage(duration=3600 * 18, interval=5)
    print("Time Tracker finished.")
