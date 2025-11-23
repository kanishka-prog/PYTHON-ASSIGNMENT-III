# PYTHON-ASSIGNMENT-III
# Library Management System
# Name       : Kanishka Saini
# Course     : Programming for Problem Solving using Python
# Roll No.   : 2501410047

class Library:
    def __init__(self, storage_file="library.txt"):
        self.storage_file = storage_file

        # Ensure file exists
        try:
            f = open(self.storage_file, "r")
            f.close()
        except:
            f = open(self.storage_file, "w")
            f.close()

    def add_book(self, book_title, book_author, book_isbn):
        with open(self.storage_file, "a") as f:
            f.write(book_title + "," + book_author + "," + book_isbn + ",available\n")
        print("Book added successfully.")

    def issue_book(self, target_isbn):
        with open(self.storage_file, "r") as f:
            content = f.readlines()

        updated_records = []
        flag = False

        for entry in content:
            fields = entry.strip().split(",")

            if fields[2] == target_isbn:
                flag = True
                if fields[3] == "available":
                    fields[3] = "issued"
                    print("Book issued successfully.")
                else:
                    print("Book is already issued.")
            updated_records.append(",".join(fields) + "\n")

        if not flag:
            print("Book not found.")
            return

        with open(self.storage_file, "w") as f:
            f.writelines(updated_records)

    def return_book(self, target_isbn):
        with open(self.storage_file, "r") as f:
            content = f.readlines()

        new_data = []
        flag = False

        for entry in content:
            fields = entry.strip().split(",")

            if fields[2] == target_isbn:
                flag = True
                if fields[3] == "issued":
                    fields[3] = "available"
                    print("Book returned successfully.")
                else:
                    print("Book is already available.")
            new_data.append(",".join(fields) + "\n")

        if not flag:
            print("Book not found.")
            return

        with open(self.storage_file, "w") as f:
            f.writelines(new_data)

    def search_by_title(self, query_title):
        with open(self.storage_file, "r") as f:
            content = f.readlines()

        found = False

        for entry in content:
            fields = entry.strip().split(",")
            if query_title.lower() in fields[0].lower():
                print("Title:", fields[0], "Author:", fields[1], "ISBN:", fields[2], "Status:", fields[3])
                found = True

        if not found:
            print("No matching book found.")

    def show_all(self):
        with open(self.storage_file, "r") as f:
            content = f.readlines()

        if not content:
            print("No books in the library.")
            return

        for entry in content:
            fields = entry.strip().split(",")
            print("Title:", fields[0], "Author:", fields[1], "ISBN:", fields[2], "Status:", fields[3])

library_system = Library()

while True:
    print("\n--- Library Menu ---")
    print("1. Add Book")
    print("2. Issue Book")
    print("3. Return Book")
    print("4. View All Books")
    print("5. Search Book By Title")
    print("6. Exit")

    user_choice = input("Enter your option: ")

    if user_choice == "1":
        t = input("Enter title: ")
        a = input("Enter author: ")
        i = input("Enter ISBN: ")
        library_system.add_book(t, a, i)

    elif user_choice == "2":
        i = input("Enter ISBN to issue: ")
        library_system.issue_book(i)

    elif user_choice == "3":
        i = input("Enter ISBN to return: ")
        library_system.return_book(i)

    elif user_choice == "4":
        library_system.show_all()

    elif user_choice == "5":
        t = input("Enter title to search: ")
        library_system.search_by_title(t)

    elif user_choice == "6":
        print("Exiting...")
        break

    else:
        print("Invalid choice, try again.")
