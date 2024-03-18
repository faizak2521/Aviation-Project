Author: Faiza Khan
```python
def prompt_flight_data():
    """
    Prompts the user for flight data and validates it.
    Returns the flight phase and current angle to the caller.
    """
    flight_status = 0  # Initializing flight status
    current_angle = 0  # Initializing current angle

    try:
        # Prompting user for flight phase input
        print("\n\t\tFlight Phases: "
              "1) Take-off, "
              "2) In-flight, "
              "3) Landing, "
              "4) Exit "
              "\n\t\t\t\tPlease enter a flight phase: ", end="")
        flight_status = int(input())  # Reading user input for flight phase
    except ValueError:
        flight_status = -1  # Setting invalid input flag if ValueError occurs

    if flight_status == 4:
        current_angle = 0  # Resetting angle if the flight is exiting

    # Validating flight phase and angle
    if flight_status != 4:
        try:
            if not 1 <= flight_status <= 3:
                current_angle = 0  # Resetting angle for invalid flight phases
            else:
                # Prompting user for current flight angle input
                print("\t\t\t\tPlease enter current flight angle: ", end="")
                current_angle = float(input())  # Reading user input for angle
        except ValueError:
            flight_status = -1  # Setting invalid input flag if ValueError occurs
            current_angle = 0  # Resetting angle for invalid input

    return flight_status, current_angle


def save_flight_data(flight_phase, angle, message):
    """
    Logs flight data into the flight_log.txt file.
    Takes flight phase, angle, and a message as input.
    """
    file_name = "flight_log.txt"
    with open(file_name, 'a') as file_handle:
        file_handle.write(f"{flight_phase}, {angle}, {message} \n")


def calculate_adjustment(phase, angle, min_pitch, max_pitch, phase_name):
    """
    Calculates adjustments during flight phases based on the provided angle.
    Guides the pilot regarding pitch adjustments.
    """
    basic_message = f"\tThe acceptable {phase_name} range is {min_pitch} – {max_pitch} degrees."

    if angle > max_pitch:
        adjustment = angle - max_pitch
        adjustment = round(adjustment, 2)
        adjust_message = f"\tNose DOWN by {adjustment} degrees."
    elif angle < min_pitch:
        adjustment = min_pitch - angle
        adjustment = round(adjustment, 2)
        adjust_message = f"\tNose UP by {adjustment} degrees."
    else:
        adjust_message = "\tMaintain your current pitch."

    print(f"\t{adjust_message}")
    print(f"\t{basic_message}")
    save_flight_data(phase, angle, adjust_message)


def main():
    """
    Main function of the Python Airways application.
    Manages the flow of the application, prompting users for flight data,
    calculating adjustments during different flight phases, and logging the data.
    """
    print(f"\n\tWelcome to Python Airways")
    print("\tYou will first enter 1) the flight phase, then 2) the current flight angle.")

    save_flight_data("Flight Beginning", 0, "Taxi-to-Runway")  # Logging flight beginning
    flight_phase, angle = prompt_flight_data()  # Prompting for flight phase and angle

    # Main flight loop
    while flight_phase != 4:
        if flight_phase == 1:
            calculate_adjustment(flight_phase, angle, 30.0, 45.0, "take-off")  # Calculating adjustments for take-off
        elif flight_phase == 2:
            calculate_adjustment(flight_phase, angle, 4.10, 5.20, "in-flight")  # Calculating adjustments for in-flight
        elif flight_phase == 3:
            calculate_adjustment(flight_phase, angle, 12.0, 25.5, "landing")  # Calculating adjustments for landing
        elif flight_phase == 4:
            save_flight_data("Flight Successful", 0, "Taxi-to-Terminal")  # Logging successful flight
            break  # Exiting loop if the flight is successful
        else:
            save_flight_data(flight_phase, angle, "Invalid data")  # Logging invalid data
            print("\nInvalid data received, please try again with a valid entry.")

        flight_phase, angle = prompt_flight_data()  # Prompting for next flight phase and angle

    print("\t\tThank you for flying Python Airways")


if __name__ == '__main__':
    main()  # Running the main function

```
