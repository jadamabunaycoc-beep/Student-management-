# A PYTHON CONSOLE-BASED STUDENT INFORMATION SYSTEM

accounts = []
students = []
attendance = []


# ========== REGISTER ACCOUNT ==========
def register():
    print("\n========== REGISTER ACCOUNT ==========")
    print("1. Student")
    print("2. Teacher")
    print("3. Registrar")

    while True:
        choice = input("Choose account type: ")

        if choice == "1":
            role = "Student"
            break
        elif choice == "2":
            role = "Teacher"
            break
        elif choice == "3":
            role = "Registrar"
            break
        else:
            print("[ERROR] Please choose 1, 2, or 3.")

    # USERNAME
    while True:
        print("\nUsername Guide:")
        print("- 5 or more characters")
        print("- Letters only")
        print("Example: MIGUEL")

        username = input("Enter Username: ")

        if len(username) < 5:
            print("[ERROR] Username must be 5 or more letters.")
            continue

        valid = True

        for char in username:
            if char not in "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz":
                valid = False

        if not valid:
            print("[ERROR] Username must contain letters only.")
            continue

        break

    # PASSWORD
    while True:
        print("\nPassword Guide:")
        print("- 6 or more characters")
        print("- Must contain at least one letter")
        print("- Must contain at least one number")
        print("- Must contain at least one symbol")
        print("Example: Miguel@123")

        password = input("Enter Password: ")

        if len(password) < 6:
            print("[ERROR] Password must be 6 or more characters.")
            continue

        has_letter = False
        has_number = False
        has_symbol = False

        for char in password:
            if char in "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz":
                has_letter = True
            elif char in "0123456789":
                has_number = True
            else:
                has_symbol = True

        if not has_letter:
            print("[ERROR] Password must contain at least one letter.")
            continue

        if not has_number:
            print("[ERROR] Password must contain at least one number.")
            continue

        if not has_symbol:
            print("[ERROR] Password must contain at least one symbol.")
            continue

        break

    for account in accounts:
        if account["username"] == username:
            print("[ERROR] Username already exists.")
            return

    accounts.append({
        "username": username,
        "password": password,
        "role": role
    })

    print("\nAccount registered successfully!")


# ========== LOG IN ==========
def login():
    print("\n========== LOG IN ==========")

    if not accounts:
        print("[ERROR] Please register an account first.")
        return

    print("1. Student Login")
    print("2. Teacher Login")
    print("3. Registrar Login")

    while True:
        choice = input("Choose login type: ")

        if choice == "1":
            role = "Student"
            break
        elif choice == "2":
            role = "Teacher"
            break
        elif choice == "3":
            role = "Registrar"
            break
        else:
            print("[ERROR] Please choose 1, 2, or 3.")

    while True:
        print("\nLogin Guide:")
        print("- Use the username and password you registered.")
        print("- Username: 5 or more letters.")
        print("- Password: 6 or more characters with letter, number, and symbol.")

        username = input("Enter Username: ")
        password = input("Enter Password: ")

        found = False

        for account in accounts:
            if (account["username"] == username
                    and account["password"] == password
                    and account["role"] == role):

                found = True

                print("\nLogin successful!")
                print("Welcome,", username)

                if role == "Student":
                    student_menu()
                elif role == "Teacher":
                    teacher_menu()
                else:
                    registrar_menu()

                return

        if not found:
            print("[ERROR] Invalid username, password, or account type.")
            print("Please try again.")


# ========== GET STUDENT ID ==========
def get_id(prompt):
    while True:
        print("\nStudent ID Guide:")
        print("- Required format: XX-XXXX-XXXXXX")
        print("- Numbers and '-' only")
        print("- Example: 02-2627-013199")
        print("- Enter 0 to go back.")

        student_id = input(prompt)

        if student_id == "0":
            return "0"

        valid = True

        if len(student_id) != 14:
            valid = False

        if valid:
            if student_id[2] != "-" or student_id[7] != "-":
                valid = False

        if valid:
            for char in student_id[:2]:
                if char not in "0123456789":
                    valid = False

            for char in student_id[3:7]:
                if char not in "0123456789":
                    valid = False

            for char in student_id[8:14]:
                if char not in "0123456789":
                    valid = False

        if valid:
            return student_id

        print("[ERROR] Invalid Student ID.")
        print("Please enter the Student ID again using the required format.")


# ========== GET NAME ==========
def get_name(prompt):
    while True:
        print("\nName Guide:")
        print("Format: SURNAME, FIRST NAME, MIDDLE INITIAL.")
        print("Example: HORMIGA, MIGUEL, N.")

        name = input(prompt)

        if name == "":
            print("[ERROR] Name cannot be empty.")
            continue

        valid = True

        # Check letters, spaces, comma, and period only
        for char in name:
            if char not in (
                "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
                "abcdefghijklmnopqrstuvwxyz ,."
            ):
                valid = False

        # Required comma
        if "," not in name:
            valid = False

        if valid:
            parts = name.split(",")

            if len(parts) != 3:
                valid = False

        if valid:
            surname = parts[0].strip()
            first_name = parts[1].strip()
            middle_initial = parts[2].strip()

            if surname == "" or first_name == "" or middle_initial == "":
                valid = False

        if valid:
            if len(middle_initial) != 2:
                valid = False

        if valid:
            if middle_initial[1] != ".":
                valid = False

        if valid:
            for char in middle_initial[0]:
                if char not in (
                    "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
                    "abcdefghijklmnopqrstuvwxyz"
                ):
                    valid = False

        if valid:
            return name

        print("[ERROR] Invalid name.")
        print("Please use the format:")
        print("SURNAME, FIRST NAME, MIDDLE INITIAL.")


# ========== GET AGE ==========
def get_age(prompt):
    while True:
        print("\nAge Guide:")
        print("- Numbers only")
        print("- Age must be greater than 0")

        value = input(prompt)

        try:
            age = int(value)

            if age > 0:
                return age

            print("[ERROR] Age must be greater than zero.")

        except ValueError:
            print("[ERROR] Please enter numbers only.")


# ========== GET BIRTHDAY ==========
def get_birthday(prompt):
    while True:
        print("\nBirthday Guide:")
        print("- Required format: YYYY-MM-DD")
        print("- Example: 2008-09-30")
        print("- Numbers only for year, month, and day.")

        birthday = input(prompt)

        valid = True

        if len(birthday) != 10:
            valid = False

        if valid:
            if birthday[4] != "-" or birthday[7] != "-":
                valid = False

        if valid:
            for char in birthday[:4]:
                if char not in "0123456789":
                    valid = False

            for char in birthday[5:7]:
                if char not in "0123456789":
                    valid = False

            for char in birthday[8:10]:
                if char not in "0123456789":
                    valid = False

        if valid:
            year = int(birthday[:4])
            month = int(birthday[5:7])
            day = int(birthday[8:10])

            if month >= 1 and month <= 12:

                days_in_month = [
                    31, 28, 31, 30, 31, 30,
                    31, 31, 30, 31, 30, 31
                ]

                if year % 400 == 0 or (
                    year % 4 == 0 and year % 100 != 0
                ):
                    days_in_month[1] = 29

                if day >= 1 and day <= days_in_month[month - 1]:
                    return birthday

        print("[ERROR] Invalid birthday.")
        print("Please enter the birthday again using YYYY-MM-DD.")


# ========== COURSE LIST ==========
def choose_course():
    courses = [
        "BS Information Technology",
        "BS Computer Science",
        "BS Computer Engineering",
        "BS Civil Engineering",
        "BS Electrical Engineering",
        "BS Mechanical Engineering",
        "BS Accountancy",
        "BS Business Administration",
        "BS Hospitality Management",
        "BS Tourism Management",
        "BS Psychology",
        "BS Nursing",
        "BS Education",
        "BS Criminology",
        "BS Social Work"
    ]

    while True:
        print("\n========== PROGRAM / COURSE ==========")

        for i in range(len(courses)):
            print(str(i + 1) + ". " + courses[i])

        print("0. Cancel")

        choice = input("Choose Program/Course (1-15): ")

        try:
            number = int(choice)

            if number == 0:
                return "0"

            if number >= 1 and number <= 15:
                return courses[number - 1]

            print("[ERROR] Please choose a number from 1-15.")

        except ValueError:
            print("[ERROR] Please enter numbers 1-15 only.")


# ========== ADD STUDENT ==========
def add_student():
    print("\n========== ADD STUDENT ==========")

    while True:
        student_id = get_id(
            "Enter Student ID (XX-XXXX-XXXXXX): "
        )

        if student_id == "0":
            return

        exists = False

        for student in students:
            if student["id"] == student_id:
                exists = True

        if exists:
            print("[ERROR] Student ID already exists.")
            print("Please enter another Student ID.")
        else:
            break

    name = get_name("Enter Student Name: ")

    age = get_age("Enter Age: ")

    birthday = get_birthday(
        "Enter Birthday (YYYY-MM-DD): "
    )

    course = choose_course()

    if course == "0":
        print("Adding student cancelled.")
        return

    students.append({
        "id": student_id,
        "name": name,
        "age": age,
        "birthday": birthday,
        "course": course
    })

    print("\nStudent added successfully!")


# ========== DISPLAY STUDENT TABLE ==========
def display_table(records):
    if not records:
        print("No student records found.")
        return

    print("=" * 110)

    print(
        f"{'STUDENT ID':<17}"
        f"{'NAME':<32}"
        f"{'AGE':<7}"
        f"{'BIRTHDAY':<15}"
        f"{'PROGRAM / COURSE':<35}"
    )

    print("=" * 110)

    for student in records:
        print(
            f"{student['id']:<17}"
            f"{student['name']:<32}"
            f"{student['age']:<7}"
            f"{student['birthday']:<15}"
            f"{student['course']:<35}"
        )

    print("=" * 110)


# ========== VIEW STUDENTS ==========
def view_students():
    print("\n========== VIEW STUDENT RECORDS ==========")
    display_table(students)


# ========== SEARCH STUDENT ==========
def search_student():
    print("\n========== SEARCH STUDENT ==========")

    while True:
        student_id = get_id(
            "Enter Student ID to search: "
        )

        if student_id == "0":
            return

        found = False

        for student in students:
            if student["id"] == student_id:
                print("\n========== SEARCH RESULT ==========")
                display_table([student])
                found = True
                return

        if not found:
            print("[ERROR] Student ID does not exist.")
            print("Please input another Student ID.")


# ========== UPDATE STUDENT ==========
def update_student():
    print("\n========== UPDATE STUDENT ==========")

    while True:
        student_id = get_id(
            "Enter Student ID to update: "
        )

        if student_id == "0":
            return

        selected_student = None

        for student in students:
            if student["id"] == student_id:
                selected_student = student

        if selected_student is not None:
            break

        print("[ERROR] Student ID does not exist.")
        print("Please input another Student ID.")

    print("\nCurrent Student Information:")
    display_table([selected_student])

    print("\nLeave blank if you do not want to change the information.")

    # ----- UPDATE NAME -----
    while True:
        print("\nName Guide:")
        print("Format: SURNAME, FIRST NAME, MIDDLE INITIAL.")
        print("Example: HORMIGA, MIGUEL, N.")

        name = input("Enter New Name: ")

        if name == "":
            break

        valid = True

        if "," not in name:
            valid = False

        for char in name:
            if char not in (
                "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
                "abcdefghijklmnopqrstuvwxyz ,."
            ):
                valid = False

        if valid:
            parts = name.split(",")

            if len(parts) != 3:
                valid = False

        if valid:
            surname = parts[0].strip()
            first_name = parts[1].strip()
            middle_initial = parts[2].strip()

            if surname == "" or first_name == "" or middle_initial == "":
                valid = False

        if valid:
            if len(middle_initial) != 2:
                valid = False

        if valid:
            if middle_initial[1] != ".":
                valid = False

        if valid:
            if middle_initial[0] not in (
                "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
                "abcdefghijklmnopqrstuvwxyz"
            ):
                valid = False

        if valid:
            selected_student["name"] = name
            break

        print("[ERROR] Invalid name.")
        print("Please enter the name again using the required format.")

    # ----- UPDATE AGE -----
    while True:
        print("\nAge Guide:")
        print("Numbers only.")
        print("Age must be greater than 0.")
        print("Leave blank to keep current age.")

        age = input("Enter New Age: ")

        if age == "":
            break

        try:
            new_age = int(age)

            if new_age > 0:
                selected_student["age"] = new_age
                break
            else:
                print("[ERROR] Age must be greater than zero.")

        except ValueError:
            print("[ERROR] Age must contain numbers only.")
            print("Please enter the age again.")

    # ----- UPDATE BIRTHDAY -----
    while True:
        print("\nBirthday Guide:")
        print("Format: YYYY-MM-DD")
        print("Example: 2008-09-30")
        print("Leave blank to keep current birthday.")

        birthday = input("Enter New Birthday: ")

        if birthday == "":
            break

        valid = True

        if len(birthday) != 10:
            valid = False

        if valid:
            if birthday[4] != "-" or birthday[7] != "-":
                valid = False

        if valid:
            for char in birthday[:4]:
                if char not in "0123456789":
                    valid = False

            for char in birthday[5:7]:
                if char not in "0123456789":
                    valid = False

            for char in birthday[8:10]:
                if char not in "0123456789":
                    valid = False

        if valid:
            year = int(birthday[:4])
            month = int(birthday[5:7])
            day = int(birthday[8:10])

            if month >= 1 and month <= 12:

                days_in_month = [
                    31, 28, 31, 30, 31, 30,
                    31, 31, 30, 31, 30, 31
                ]

                if year % 400 == 0 or (
                    year % 4 == 0 and year % 100 != 0
                ):
                    days_in_month[1] = 29

                if day >= 1 and day <= days_in_month[month - 1]:
                    selected_student["birthday"] = birthday
                    break

        print("[ERROR] Invalid birthday.")
        print("Please enter the birthday again using YYYY-MM-DD.")

    # ----- UPDATE COURSE -----
    while True:
        print("\nCurrent Program/Course:")
        print(selected_student["course"])

        print("\nChoose a new Program/Course:")
        print("Leave blank to keep current course.")

        course_choice = input(
            "Enter Program/Course number (1-15): "
        )

        if course_choice == "":
            break

        try:
            number = int(course_choice)

            if number >= 1 and number <= 15:

                courses = [
                    "BS Information Technology",
                    "BS Computer Science",
                    "BS Computer Engineering",
                    "BS Civil Engineering",
                    "BS Electrical Engineering",
                    "BS Mechanical Engineering",
                    "BS Accountancy",
                    "BS Business Administration",
                    "BS Hospitality Management",
                    "BS Tourism Management",
                    "BS Psychology",
                    "BS Nursing",
                    "BS Education",
                    "BS Criminology",
                    "BS Social Work"
                ]

                selected_student["course"] = courses[number - 1]
                break

            else:
                print("[ERROR] Please choose a number from 1-15.")

        except ValueError:
            print("[ERROR] Please enter a course number from 1-15.")

    print("\n========== STUDENT UPDATED SUCCESSFULLY ==========")
    display_table([selected_student])


# ========== DELETE STUDENT ==========
def delete_student():
    print("\n========== DELETE STUDENT ==========")

    while True:
        student_id = get_id(
            "Enter Student ID to delete: "
        )

        if student_id == "0":
            return

        selected_student = None

        for student in students:
            if student["id"] == student_id:
                selected_student = student

        if selected_student is not None:
            break

        print("[ERROR] Student ID does not exist.")
        print("Please input another Student ID.")

    display_table([selected_student])

    while True:
        confirm = input(
            "Delete this student? (Y/N): "
        )

        if confirm == "Y" or confirm == "y":
            students.remove(selected_student)
            print("Student deleted successfully.")
            return

        elif confirm == "N" or confirm == "n":
            print("Delete cancelled.")
            return

        else:
            print("[ERROR] Please enter Y or N.")


# ========== STUDENT COUNT ==========
def student_count():
    print("\n========== STUDENT COUNT ==========")
    print("Total S