# Hangman-Game-python
import requests

def rnd_wrd():
    response = requests.get("https://random-word-api.herokuapp.com/word?number=1")
    if response.status_code == 200:
        return response.json()[0]
    else:
        print("No connection! Returning The default word!")
        return "hangman"


x = rnd_wrd().lower()
print("Selected word is:", x)
print("Let's see if you can actually get it!")

correct_guesses = []
wrong_guesses = []
max_attempts = 7

hangman_stages = [
    """
------
|    |
|
|
|
|
|
|___
""",
    """
------
|    |
|    O
|
|
|
|
|___
""",
    """
------
|    |
|    O
|    |
|
|
|
|___
""",
    """
------
|    |
|    O
|   /|
|
|
|
|___
""",
    """
------
|    |
|    O
|   /|\\
|
|
|
|___
""",
    """
------
|    |
|    O
|   /|\\
|   /
|
|
|___
""",
    """
------
|    |
|    O
|   /|\\
|   / \\
|
|
|___
"""
]

while len(wrong_guesses) < max_attempts:
    guessed_word = ""
    for letter in x:
        if letter in correct_guesses:
            guessed_word += letter
        else:
            guessed_word += "_"

    print("Word to guess: ", guessed_word)

    if guessed_word == x:
        print("Congratulations! You passed the test xd")
        break

    print("Wrong guesses: ", wrong_guesses)
    print(hangman_stages[len(wrong_guesses)])
    guess = input("Please guess a letter: ").lower()

    if guess in x:
        print('You got it!')
        correct_guesses.append(guess)
    else:
        print("Incorrect guess!")
        wrong_guesses.append(guess)
        remaining_attempts = max_attempts - len(wrong_guesses)
        print("Attempts remaining:", remaining_attempts)
        if remaining_attempts == 0:
            print("Game over. Play again!")
            break
        else:
            print("Nope")

