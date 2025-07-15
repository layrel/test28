# Урок №10. Задание №1 вы создавали словарь с информацией о питомце. Теперь нам нужна "настоящая" база данных для ветеринарной клиники.  Подробный требуемый функционал будет ниже. Пока что справка:      Создайте функцию create Создайте функцию read     Создайте функцию update     Создайте функцию delete   
# Решение:
import collections
pets = {
    1: {
        "Мухтар": {
            "Вид питомца": "Собака",
            "Возраст питомца": 9,
            "Имя владельца": "Павел"
        },
    },
    2: {
        "Каа": {
            "Вид питомца": "желторотый питон",
            "Возраст питомца": 19,
            "Имя владельца": "Саша"
        },
    },
}

def get_pet(ID):
    return pets[ID] if ID in pets.keys() else False
def get_suffix(age):
    if age % 10 == 1 and age % 100 != 11:
        return 'год'
    elif 2 <= age % 10 <= 4 and not (12 <= age % 100 <= 14):
        return 'года'
    else:
        return 'лет'

def pets_list():
    for pet_id, pet_info in pets.items():
        for name, details in pet_info.items():
            age_suffix = get_suffix(details["Возраст питомца"])
            print(f"Это {details['Вид питомца']} по кличке \"{name}\". Возраст питомца: {details['Возраст питомца']} {age_suffix}. Имя владельца: {details['Имя владельца']}.")

def create():
    last = collections.deque(pets, maxlen=1)[0]
    new_id = last + 1

    name = input("Введите кличку питомца: ")
    kind = input("Введите вид питомца: ")
    age = int(input("Введите возраст питомца: "))
    owner = input("Введите имя владельца: ")

    pets[new_id] = {
        name: {
            "Вид питомца": kind,
            "Возраст питомца": age,
            "Имя владельца": owner
        },
    }
    print(f"Питомец {name} успешно добавлен!")

def read():
    pet_id = int(input("Введите ID питомца, информацию о котором хотите посмотреть: "))
    pet = get_pet(pet_id)

    if pet:
        for name, details in pet.items():
            age_suffix = get_suffix(details["Возраст питомца"])
            print(f"Это {details['Вид питомца']} по кличке \"{name}\". Возраст питомца: {details['Возраст питомца']} {age_suffix}. Имя владельца: {details['Имя владельца']}.")
    else:
        print("Питомца с таким ID нет.")

def update():
    pet_id = int(input("Введите ID питомца для обновления: "))
    pet = get_pet(pet_id)

    if pet:
        name = list(pet.keys())[0]
        kind = input(f"Введите новый вид питомца (текущий: {pet[name]['Вид питомца']}): ") or pet[name]["Вид питомца"]
        age = int(input(f"Введите новый возраст питомца (текущий: {pet[name]['Возраст питомца']}): ") or pet[name]["Возраст питомца"])
        owner = input(f"Введите новое имя владельца (текущее: {pet[name]['Имя владельца']}): ") or pet[name]["Имя владельца"]

        pets[pet_id][name]["Вид питомца"] = kind
        pets[pet_id][name]["Возраст питомца"] = age
        pets[pet_id][name]["Имя владельца"] = owner
        print(f"Информация о питомце {name} успешно обновлена!")
    else:
        print("Питомца с таким ID нет.")

def delete():
    pet_id = int(input("Введите ID питомца, которого хотите удалить: "))
    if pet_id in pets:
        del pets[pet_id]
        print("Питомец успешно удален!")
    else:
        print("Питомца с таким ID нет.")
while True:
    command = input("Введите команду (create, read, update, delete, list, stop): ").strip().lower()

    if command == 'create':
        create()
    elif command == 'read':
        read()
    elif command == 'update':
        update()
    elif command == 'delete':
        delete()
    elif command == 'list':
        pets_list()
    elif command == 'stop':
        print("Программа завершена.")
        break
    else:
        print("Неизвестная команда. Пожалуйста, попробуйте снова.")
