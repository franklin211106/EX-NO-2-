## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
  
To write a python program to implement the Playfair Substitution technique.

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

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




# Program:
# Function to clean and prepare the text
def prepare_text(text):
    text = text.upper().replace("J", "I")  # Replace 'J' with 'I' (common convention in Playfair cipher)
    prepared_text = ""
    
    i = 0
    while i < len(text):
        if i + 1 < len(text):
            if text[i] == text[i + 1]:
                prepared_text += text[i] + 'X'  # If two characters are same, add 'X' between them
                i += 1
            else:
                prepared_text += text[i] + text[i + 1]
                i += 2
        else:
            prepared_text += text[i] + 'X'  # Add 'X' if only one character left
            i += 1

    return prepared_text

# Function to generate the 5x5 matrix (Playfair cipher key)
def generate_matrix(key):
    key = key.upper().replace("J", "I")  # Replace 'J' with 'I'
    matrix = []
    seen = set()
    
    # Add the key characters to the matrix
    for char in key:
        if char not in seen:
            seen.add(char)
            matrix.append(char)
    
    # Fill the rest of the matrix with the remaining letters of the alphabet
    for char in "ABCDEFGHIKLMNOPQRSTUVWXYZ":
        if char not in seen:
            seen.add(char)
            matrix.append(char)
    
    return matrix

# Function to create the matrix grid from the generated key
def create_matrix_grid(matrix):
    grid = []
    for i in range(5):
        grid.append(matrix[i*5:(i+1)*5])
    return grid

# Function to find the position of a letter in the matrix
def find_position(char, grid):
    for row in range(5):
        for col in range(5):
            if grid[row][col] == char:
                return row, col

# Function to encrypt a pair of letters
def encrypt_pair(a, b, grid):
    row1, col1 = find_position(a, grid)
    row2, col2 = find_position(b, grid)

    if row1 == row2:
        return grid[row1][(col1 + 1) % 5] + grid[row2][(col2 + 1) % 5]
    elif col1 == col2:
        return grid[(row1 + 1) % 5][col1] + grid[(row2 + 1) % 5][col2]
    else:
        return grid[row1][col2] + grid[row2][col1]

# Function to encrypt the plaintext using the Playfair cipher
def playfair_encrypt(plaintext, key):
    plaintext = prepare_text(plaintext)
    matrix = generate_matrix(key)
    grid = create_matrix_grid(matrix)
    
    ciphertext = ""
    for i in range(0, len(plaintext), 2):
        pair = plaintext[i:i+2]
        ciphertext += encrypt_pair(pair[0], pair[1], grid)
    
    return ciphertext

# Function to decrypt a pair of letters
def decrypt_pair(a, b, grid):
    row1, col1 = find_position(a, grid)
    row2, col2 = find_position(b, grid)

    if row1 == row2:
        return grid[row1][(col1 - 1) % 5] + grid[row2][(col2 - 1) % 5]
    elif col1 == col2:
        return grid[(row1 - 1) % 5][col1] + grid[(row2 - 1) % 5][col2]
    else:
        return grid[row1][col2] + grid[row2][col1]

# Function to decrypt the ciphertext using the Playfair cipher
def playfair_decrypt(ciphertext, key):
    matrix = generate_matrix(key)
    grid = create_matrix_grid(matrix)
    
    plaintext = ""
    for i in range(0, len(ciphertext), 2):
        pair = ciphertext[i:i+2]
        plaintext += decrypt_pair(pair[0], pair[1], grid)
    
    return plaintext

# Example usage
if __name__ == "__main__":
    key = "KEYWORD"
    plaintext = "HELLO"

    # Encryption
    ciphertext = playfair_encrypt(plaintext, key)
    print(f"Ciphertext: {ciphertext}")

    # Decryption
    decrypted_text = playfair_decrypt(ciphertext, key)
    print(f"Decrypted Text: {decrypted_text}")


# Output:
![Screenshot 2025-03-20 092442](https://github.com/user-attachments/assets/fc5ee485-13e4-4d37-88be-3884e9338dbd)




