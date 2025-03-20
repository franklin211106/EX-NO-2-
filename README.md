## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
To write a python program Function to create the key matrix

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

### STEP-1: Read the plain text from the user.
### STEP-2: Read the keyword from the user.
### STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
### STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
### STEP-5: Display the obtained cipher text.




## Program:
# Function to create the key matrix
def create_key_matrix(keyword):
    matrix = []
    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"  # I and J are combined into one slot
    used_chars = set()

    # Remove duplicates and add keyword to matrix
    for char in keyword.upper():
        if char not in used_chars and char != 'J':
            matrix.append(char)
            used_chars.add(char)

    # Add remaining letters of the alphabet
    for char in alphabet:
        if char not in used_chars:
            matrix.append(char)
            used_chars.add(char)

    # Create a 5x5 matrix
    return [matrix[i:i + 5] for i in range(0, len(matrix), 5)]

# Function to prepare the plaintext
def prepare_text(plaintext):
    prepared = ""
    i = 0
    while i < len(plaintext):
        # If two consecutive characters are the same, add 'X' between them
        if i + 1 < len(plaintext) and plaintext[i] == plaintext[i + 1]:
            prepared += plaintext[i] + 'X'
            i += 1
        elif i + 1 < len(plaintext):
            prepared += plaintext[i] + plaintext[i + 1]
            i += 2
        else:
            prepared += plaintext[i] + 'X'  # Add 'X' if odd length
            i += 1
    return prepared

# Function to find the position of a character in the key matrix
def find_position(char, matrix):
    for i in range(5):
        for j in range(5):
            if matrix[i][j] == char:
                return i, j
    return None

# Function to encrypt the text using Playfair cipher
def playfair_encrypt(plaintext, keyword):
    matrix = create_key_matrix(keyword)
    plaintext = prepare_text(plaintext)
    ciphertext = ""

    for i in range(0, len(plaintext), 2):
        char1 = plaintext[i].upper()
        char2 = plaintext[i + 1].upper()

        row1, col1 = find_position(char1, matrix)
        row2, col2 = find_position(char2, matrix)

        # Case 1: Same row
        if row1 == row2:
            ciphertext += matrix[row1][(col1 + 1) % 5]
            ciphertext += matrix[row2][(col2 + 1) % 5]
        # Case 2: Same column
        elif col1 == col2:
            ciphertext += matrix[(row1 + 1) % 5][col1]
            ciphertext += matrix[(row2 + 1) % 5][col2]
        # Case 3: Rectangle
        else:
            ciphertext += matrix[row1][col2]
            ciphertext += matrix[row2][col1]

    return ciphertext

# Function to decrypt the text using Playfair cipher
def playfair_decrypt(ciphertext, keyword):
    matrix = create_key_matrix(keyword)
    plaintext = ""

    for i in range(0, len(ciphertext), 2):
        char1 = ciphertext[i].upper()
        char2 = ciphertext[i + 1].upper()

        row1, col1 = find_position(char1, matrix)
        row2, col2 = find_position(char2, matrix)

        # Case 1: Same row
        if row1 == row2:
            plaintext += matrix[row1][(col1 - 1) % 5]
            plaintext += matrix[row2][(col2 - 1) % 5]
        # Case 2: Same column
        elif col1 == col2:
            plaintext += matrix[(row1 - 1) % 5][col1]
            plaintext += matrix[(row2 - 1) % 5][col2]
        # Case 3: Rectangle
        else:
            plaintext += matrix[row1][col2]
            plaintext += matrix[row2][col1]

    return plaintext

# Test the Playfair Cipher
if __name__ == "__main__":
    plaintext = input("Enter the plaintext: ").replace(" ", "")
    keyword = input("Enter the keyword: ").replace(" ", "")
    
    encrypted_text = playfair_encrypt(plaintext, keyword)
    print(f"Encrypted Text: {encrypted_text}")
    
    decrypted_text = playfair_decrypt(encrypted_text, keyword)
    print(f"Decrypted Text: {decrypted_text}")



Output:

![Screenshot 2025-03-20 092442](https://github.com/user-attachments/assets/eb9608d7-9de3-4c8a-b28b-f47a3efbeca4)


