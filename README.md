# FLight Project
# Airplane Aviation Project in Python: Developed a Python project focusing on airplane aviation, utilizing object-oriented programming principles to simulate real-world scenarios.
# Python Airways Project
# Author: Faiza Khan

def data_prompt():
    """
    This function prompts the user for flight data and validates it.
    It returns the flight phase and current angle to the caller.
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
            if flight_status < 1 or flight_status > 3:
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
    This function logs flight data into the flight_log.txt file.
    It takes flight phase, angle, and a message as input.
    """

    file_name = "flight_log.txt"
    with open(file_name, 'a') as file_handle:
        file_handle.write(f"{flight_phase}, {angle}, {message} \n")


def take_off_calculation(angle):
    """
    This function calculates adjustments during the take-off phase based on the provided angle.
    It guides the pilot regarding pitch adjustments.
    """

    basic_message = "\tThe acceptable take-off pitch range is 30 – 45 degrees."
    min_pitch = 30.00
    max_pitch = 45.00

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
    save_flight_data("take-off", angle, adjust_message)


def in_flight_calculation(angle):
    """
    This function calculates adjustments during the in-flight phase based on the provided angle.
    It guides the pilot regarding pitch adjustments.
    """

    basic_message = "\tThe acceptable in-flight angle/pitch range is 4.1 – 5.2 degrees."
    min_pitch = 4.10
    max_pitch = 5.20

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
    save_flight_data("in-flight", angle, adjust_message)


def landing_calculation(angle):
    """
    This function calculates adjustments during the landing phase based on the provided angle.
    It guides the pilot regarding pitch adjustments.
    """

    basic_message = "\tThe acceptable landing pitch/range is 12.0 – 25.5 degrees."
    min_pitch = 12.00
    max_pitch = 25.50

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
    save_flight_data("landing", angle, adjust_message)


def main():
    """
    This is the main function of the Python Airways application.
    It manages the flow of the application, prompting users for flight data,
    calculating adjustments during different flight phases, and logging the data.
    """

    print(f"\n\tWelcome to Python Airways")
    print("\tYou will first enter 1) the flight phase, then 2) the current flight angle.")

    user_selection = ""  # Variable for user input
    previous_selection = 0  # Variable for tracking previous flight phase

    save_flight_data("Flight Beginning", 0, "Taxi-to-Runway")  # Logging flight beginning
    flight_phase, angle = data_prompt()  # Prompting for flight phase and angle

    # Main flight loop
    while flight_phase != 4:
        if flight_phase >= previous_selection and flight_phase == 1:
            take_off_calculation(angle)  # Calculating adjustments for take-off
            previous_selection = flight_phase
        elif flight_phase >= previous_selection and flight_phase == 2:
            in_flight_calculation(angle)  # Calculating adjustments for in-flight
            previous_selection = flight_phase
        elif flight_phase >= previous_selection and flight_phase == 3:
            landing_calculation(angle)  # Calculating adjustments for landing
            previous_selection = flight_phase
        elif flight_phase == 4:
            save_flight_data("Flight Successful", 0, "Taxi-to-Terminal")  # Logging successful flight
            break  # Exiting loop if flight is successful
        else:
            save_flight_data(flight_phase, angle, "Invalid data")  # Logging invalid data
            print("\nInvalid data received, please try again with a valid entry.")

        flight_phase, angle = data_prompt()  # Prompting for next flight phase and angle

    print("\t\tThank you for flying Python Airways")


if __name__ == '__main__':
    main()  # Running the main function
