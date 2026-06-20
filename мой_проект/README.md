import random
import os

def play_hangman():
    words = []
    if os.path.exists("words.txt"):
        try:
            with open("words.txt", "r", encoding="utf-8") as file:
                content = file.read().strip()
                if content:
                    words = [word.strip().lower() for word in content.splitlines()]
        except Exception as e:
            print(f"Ошибка чтения файла: {e}")

    secret_word = random.choice(words).lower()
    result = set()
    max_attempts = 6
    attempts = 0
    print("Добро пожаловать в игру Виселица!")
    print(f"У вас есть {max_attempts} попыток, чтобы угадать слово.")

    while attempts < max_attempts:
        if all(l in result for l in secret_word):
            print(f"Вы угадали слово {secret_word}!")
            return

        current_display = ''
        for letter in secret_word:
            current_display += letter if letter in result else '_'
        print(f"Слово: {current_display}")

        letter = input("Введите букву: ").lower()
        if len(letter) != 1 or not letter.isalpha():
            print("Введите одну букву")
            continue
        if letter in result:
            print("Вы уже вводили эту букву")
            continue

        result.add(letter)
        if letter in secret_word:
            print(f"Буква '{letter}' есть в слове!")
        else:
            attempts += 1
            print(f"Буквы '{letter}' нет в слове. Использована {attempts} попытка")

    print(f"Вы проиграли! Загаданное слово было: '{secret_word}'")

while True:
    play_hangman()
    while True:
        play_again = input("Хотите сыграть ещё раз? да/нет: ").lower().strip()
        if play_again in ["да", "нет"]:
            break
        print("Введите, пожалуйста, 'да' или 'нет'")
    if play_again == "нет":
        print("Спасибо за игру!")
        break
