## Design Reservation and Payment System for Parking Garage

- Product Requirements
  - Need to be able to reserve a parking spot and receive ticket or receipt
  - Need to be able to pay for a parking spot
  - High consistency: no two people should be able to reserve the same spot
  - Type of parking: compact, regular, and large
  - Prices for the parking: flat rate based on time
  - web or mobile app interacting with backend server

- HLD
  - Public API endpoints
    - reserve/add
      - input:
        - parking garage
        - vehicle number
        - credit card
        - duration
      - output: 
        - spot number
        - reservation id
    - payment: taken care by 3rd party
      - input:
        - reservation id
    - cancel:
      - input:
        - reservation id
  - Internal endpoints: which can be part of the public
    - calculate payment
    - check the available spot
    - allocate the spot
    - create account: email, password, first and last name
    - login

  - Web -> backend server -> db (replicas, write/read lock)
  - Web -> backend server -> 3rd party


- DB design:
  - garage -> name, id, zip
  - cost -> garage_id, spot type, cost 
  - spot -> number, type, garage_id
  - reservation -> garage, spot, id, start, end, account, paid
  - account -> id, name, email, password
  - vehicle -> id, account_id, type, license