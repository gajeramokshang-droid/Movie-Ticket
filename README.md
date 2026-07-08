# 🎟️ Streamlit Movie Ticket Booking System

A modern, light-weight, and interactive **Movie Ticket Booking System** built using **Streamlit** for the user interface and **SQLite** for database management. The application features custom dark-themed styling, interactive seat selection, a mock payment portal, and a comprehensive admin panel for movie and theater management.

---

## 🚀 Features

### 👤 User Panel
* **Interactive Movie Details:** View genre, duration, synopsis, and cast lists.
* **Showtime Selection:** Dynamically browse available showtimes, prices, and seats remaining.
* **Interactive Seat Grid:** A visual grid-based seat selector (10 seats per row). Seats are marked as booked (disabled) or available, updating in real-time.
* **Mock Payment Portal:**
  * Supports multiple payment channels: **UPI**, **Credit/Debit Card**, and **Net Banking**.
  * Built-in input validation (e.g., card formats, UPI `@` syntax, bank options).
  * Processes bookings and displays a detailed transactional receipt.

### 📊 Admin Panel
* **Live Dashboard Metrics:** Displays aggregate analytics including **Total Bookings** and **Total Revenue**.
* **Booking Reports:** Search, filter, and view detailed booking sheets grouped by movie and showtime.
* **Movie Inventory Management:** Easily **Add** or **Delete** movies from the database.
* **Showtime Scheduling:** Add showtimes with strict validation format (`YYYY-MM-DD HH:MM AM/PM`).
* **Availability Controls:** Adjust pricing and available seats for individual showtimes dynamically.

---

## 🛠️ Database Schema

The database `movies.db` is built with SQLite and contains four core tables:

### 1. `Movies`
| Column Name | Type | Key / Attribute |
| :--- | :--- | :--- |
| `movie_id` | `INTEGER` | `PRIMARY KEY AUTOINCREMENT` |
| `title` | `TEXT` | `NOT NULL` |
| `genre` | `TEXT` | |
| `duration` | `INTEGER` | In minutes |
| `info` | `TEXT` | Synopsis / Description |
| `cast` | `TEXT` | Cast members (comma-separated) |

### 2. `Showtimes`
| Column Name | Type | Key / Attribute |
| :--- | :--- | :--- |
| `showtime_id` | `INTEGER` | `PRIMARY KEY AUTOINCREMENT` |
| `movie_id` | `INTEGER` | `FOREIGN KEY REFERENCES Movies(movie_id)` |
| `time` | `TEXT` | Formatted text representation |
| `available_seats` | `INTEGER` | |
| `price` | `REAL` | Cost per ticket (Default: `250.00`) |

### 3. `Seats`
| Column Name | Type | Key / Attribute |
| :--- | :--- | :--- |
| `seat_id` | `INTEGER` | `PRIMARY KEY AUTOINCREMENT` |
| `showtime_id` | `INTEGER` | `FOREIGN KEY REFERENCES Showtimes(showtime_id)` |
| `seat_number` | `TEXT` | e.g. `S1`, `S2` |
| `is_booked` | `INTEGER` | `DEFAULT 0` (0 = Available, 1 = Booked) |

### 4. `Bookings`
| Column Name | Type | Key / Attribute |
| :--- | :--- | :--- |
| `booking_id` | `INTEGER` | `PRIMARY KEY AUTOINCREMENT` |
| `showtime_id` | `INTEGER` | `FOREIGN KEY REFERENCES Showtimes(showtime_id)` |
| `customer_name`| `TEXT` | |
| `num_tickets` | `INTEGER` | |
| `total_price` | `REAL` | |
| `seats` | `TEXT` | Comma-separated list of seat numbers |

---

## 📂 File Architecture

* **[app.py](file:///c:/Users/gajer/OneDrive/Desktop/Movie-Ticket/app.py)**: Manages database connections with multi-threading compatibility (`check_same_thread=False`).
* **[main.py](file:///c:/Users/gajer/OneDrive/Desktop/Movie-Ticket/main.py)**: The primary entrypoint hosting the UI, dark styling configurations, user routing, and payment layout.
* **[booking.py](file:///c:/Users/gajer/OneDrive/Desktop/Movie-Ticket/booking.py)**: Core booking business logic (fetching movies/showtimes, transaction processing, seat initialization).
* **[admin.py](file:///c:/Users/gajer/OneDrive/Desktop/Movie-Ticket/admin.py)**: Houses the admin workspace containing stats, reporting, movie managers, and showtime schedules.
* **[create_db.py](file:///c:/Users/gajer/OneDrive/Desktop/Movie-Ticket/create_db.py)**: Initializes the schema structure and seeds initial default database values.
* **Migrators & Updaters**:
  * **[update_db.py](file:///c:/Users/gajer/OneDrive/Desktop/Movie-Ticket/update_db.py)**: Adds `price` column to `Showtimes` table.
  * **[update_bookings.py](file:///c:/Users/gajer/OneDrive/Desktop/Movie-Ticket/update_bookings.py)**: Adds `seats` column to `Bookings` table.
  * **[update_movies_table.py](file:///c:/Users/gajer/OneDrive/Desktop/Movie-Ticket/update_movies_table.py)**: Adds `info` and `cast` columns to `Movies` table.

---

## 💻 Setup & Installation Instructions

### Prerequisites
* Python 3.8+
* `pip` package manager

### 1. Clone & Navigate to Directory
```bash
cd Movie-Ticket
```

### 2. Install Dependencies
Make sure you have Streamlit installed:
```bash
pip install streamlit
```

### 3. Initialize the Database
Run the setup files to configure database tables and columns:
```bash
python create_db.py
python update_db.py
python update_bookings.py
python update_movies_table.py
```

### 4. Run the Application
Start the Streamlit application server:
```bash
streamlit run main.py
```
After execution, a browser tab should automatically open at `http://localhost:8501`.
