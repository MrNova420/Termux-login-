#!/bin/bash

# File to store user credentials
USER_FILE="users.txt"

# Function to display the login page
login_page() {
    clear
    echo "====================================="
    echo "         Welcome to Termux Custom login environment     "
    echo "====================================="
    echo
    echo "   Secure your terminal environment with a custom interface and built in Hacking Tools    For educational Purposes   "
    echo
    echo "====================================="
    echo

    # Prompt for login or register
    echo "1. Login"
    echo "2. Register"
    echo -n "Choose an option: "
    read option

    case $option in
        1) 
            login
            ;;
        2)
            register
            ;;
        *)
            echo "Invalid option. Please try again."
            sleep 2
            login_page
            ;;
    esac
}

# Function to handle user login
login() {
    echo -n "Enter Username: "
    read username
    echo -n "Enter Password: "
    read -s password
    echo

    # Check if username and password exist in the user file
    if grep -q "$username:$password" "$USER_FILE"; then
        echo -e "\nLogin Successful!"
        sleep 1
        main_interface
    else
        echo -e "\nInvalid Username or Password. Try again."
        sleep 2
        login_page
    fi
}

# Function to handle user registration
register() {
    echo -n "Enter a new username: "
    read new_username
    echo -n "Enter a new password: "
    read -s new_password
    echo

    # Check if the username already exists
    if grep -q "$new_username" "$USER_FILE"; then
        echo "Username already exists. Please try again."
        sleep 2
        login_page
    else
        # Save new user credentials to the user file
        echo "$new_username:$new_password" >> "$USER_FILE"
        echo "User registered successfully!"
        sleep 2
        login_page
    fi
}

# Function to display the custom interface
main_interface() {
    while true; do
        clear
        echo "====================================="
        echo "       Custom Termux login & Interface with Tools      "
        echo "                  By MrNova420                                               "
        echo "====================================="
        echo
        echo "1. Update and Upgrade Packages"
        echo "2. Display System Information"
        echo "3. Open a New Terminal Session"
        echo "4. Clear Cache"
        echo "5. Exit"
        echo "====================================="
        echo
        echo -n "Choose an option: "
        read choice

        case $choice in
            1)
                echo "Updating and upgrading packages..."
                pkg update && pkg upgrade -y
                echo "Packages updated successfully!"
                sleep 2
                ;;
            2)
                echo "System Information:"
                uname -a
                echo "CPU Info:"
                cat /proc/cpuinfo | head -10
                echo "Memory Info:"
                free -h
                echo
                echo "Press Enter to return to the main menu."
                read
                ;;
            3)
                echo "Opening a new terminal session..."
                termux-open-session
                sleep 2
                ;;
            4)
                echo "Clearing cache..."
                rm -rf ~/cache/*
                echo "Cache cleared."
                sleep 2
                ;;
            5)
                echo "Exiting... Goodbye!"
                exit 0
                ;;
            *)
                echo "Invalid option. Please try again."
                sleep 2
                ;;
        esac
    done
}

# Check if user file exists, if not create it
if [ ! -f "$USER_FILE" ]; then
    touch "$USER_FILE"
fi

# Start the login process
login_page
