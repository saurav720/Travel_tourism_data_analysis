# Travel_tourism_data_analysis


Travel & Tourism Relational Dataset

A synthetic, multi-table dataset for practising Python / Pandas: data cleaning, joins/merges, feature engineering, EDA and visualisation on a realistic travel-booking schema.

Dataset Overview
File	Rows	Columns	Description
customers.csv	1,000	15	Customer profile, location, membership tier, travel preference
bookings.csv	2,000	16	Booking dates, type, status, amount, discount, channel
flights.csv	80	15	Airline, route, duration, stops, class, fare, on-time score
hotels.csv	100	15	Hotel type, star rating, price per night, amenities, guest rating
destinations.csv	30	12	City, region, type, best season, daily budget, rating
payments.csv	2,000	12	Payment method, status, amount paid, refund, gateway
🔗 Table Relationships
customers ──< bookings >── destinations
                │   │            │   │
                │   └─ flights ──┘   └─ hotels
                │
             payments
Foreign Key	References
bookings.customer_id	customers.customer_id
bookings.destination_id	destinations.destination_id
bookings.flight_id	flights.flight_id
bookings.hotel_id	hotels.hotel_id
payments.booking_id	bookings.booking_id
flights.destination_id	destinations.destination_id
hotels.destination_id	destinations.destination_id

All foreign keys are valid (no orphan records).

🧾 Key Columns

customers – customer_id, age, gender, city, state, signup_date, membership_tier (Basic / Silver / Gold / Platinum), preferred_travel_type, total_trips, newsletter_subscribed

bookings – booking_date, travel_start_date, travel_end_date, booking_type (Flight / Hotel / Flight + Hotel), booking_status (Confirmed / Completed / Cancelled / Pending), number_of_travellers, number_of_nights, total_amount, discount_amount, booking_channel, special_request

flights – airline, source_city, destination_id, duration_hours, stops, travel_class, base_fare, on_time_score

hotels – hotel_name, star_rating, hotel_type, room_type, price_per_night, breakfast_included, pool_available, guest_rating

destinations – city, country, region, destination_type, best_season, avg_daily_budget, rating, annual_visitors_lakh

payments – payment_date, payment_method (UPI / Credit Card / Debit Card / Net Banking / Wallet), payment_status, amount_paid, refund_amount, gateway, installment_count

 Known Data Quirks

I checked the data and found the following. These are not necessarily bugs to hide — they make the dataset good practice for data validation and cleaning:

Issue	What was found
Missing values	Only bookings.special_request has nulls (384 of 2,000 rows)
Date logic	travel_end_date is before travel_start_date in 889 of 2,000 bookings
Nights mismatch	number_of_nights does not match the date difference in almost all rows
Booking vs. flight/hotel destination	flight_id / hotel_id usually belong to a different destination than the booking's destination_id (~96%)
Flight/hotel IDs	Filled for every booking, even when booking_type doesn't include that component
Geography	City–country pairs are not realistic (e.g. Jaipur → Thailand, Manali → UAE)
Customer geography	City–state pairs are not realistic (e.g. Bhopal → Rajasthan) and all customers are from India
Flight numbers	Flight-number prefix does not match the airline (e.g. SpiceJet → QR6496)
Currency	Payments and destinations use INR throughout

 Practice Ideas

Data cleaning & validation

Detect and fix bookings where the end date is before the start date
Recompute number_of_nights from the dates
Handle missing special_request values (e.g. fill with "None")
Convert date columns with pd.to_datetime() and extract year / month / weekday

Joins & merges

Merge bookings with customers, destinations and payments
Find bookings where flight/hotel destination ≠ booking destination

EDA questions

Which destinations and booking channels generate the most revenue?
What is the cancellation rate by booking type or membership tier?
Does membership tier affect average spend or discount?
Which payment method has the highest failure rate?
Monthly booking trend and seasonality
Relationship between star rating and price per night

Visualisation & modelling (beginner level)

Bar, line, box and heatmap plots with Matplotlib / Seaborn
Predict booking_status (cancelled vs. not) with a simple classifier
Customer segmentation using total_trips, spend and tier
 Quick Start
bash
pip install pandas numpy matplotlib seaborn jupyter
python
import pandas as pd

customers    = pd.read_csv("customers.csv")
bookings     = pd.read_csv("bookings.csv", parse_dates=["booking_date", "travel_start_date", "travel_end_date"])
flights      = pd.read_csv("flights.csv")
hotels       = pd.read_csv("hotels.csv")
destinations = pd.read_csv("destinations.csv")
payments     = pd.read_csv("payments.csv")

# Example: bookings + customer + destination + payment in one table

df = (bookings
      .merge(customers, on="customer_id", how="left")
      .merge(destinations, on="destination_id", how="left", suffixes=("", "_dest"))
      .merge(payments, on="booking_id", how="left"))

print(df.shape)
 Repository Structure
├── data/
│   ├── customers.csv
│   ├── bookings.csv
│   ├── flights.csv
│   ├── hotels.csv
│   ├── destinations.csv
│   └── payments.csv
├── notebook.ipynb      # your analysis
└── README.md
 License & Usage

Synthetic data created for learning purposes. Free to use for practice, portfolio projects and teaching.

 Author

saurav singh mandrawal

GitHub:[github.com/saurav720](https://github.com/saurav720)
LinkedIn: [www.linkedin.com/in/saurav-singh05](https://www.linkedin.com/in/saurav-singh05/)
