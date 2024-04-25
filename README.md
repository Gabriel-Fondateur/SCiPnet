# SCiPnet
Le SCiPnet est un site spécialement conçus pour contenire des information pour la Fondation
import json
import os
import hashlib

# Chemin du fichier de la base de données
DATABASE_FILE = "scipnet.json"
PASSWORD_FILE = "password.txt"

# Fonction pour charger les utilisateurs
def load_users():
    if os.path.exists(DATABASE_FILE):
        with open(DATABASE_FILE, "r") as file:
            return json.load(file)
    else:
        return {}

# Fonction pour sauvegarder les utilisateurs
def save_users(users):
    with open(DATABASE_FILE, "w") as file:
        json.dump(users, file, indent=4)

# Fonction pour charger le mot de passe
def load_password():
    if os.path.exists(PASSWORD_FILE):
        with open(PASSWORD_FILE, "r") as file:
            return file.read().strip()
    else:
        return hashlib.sha256("SCiPnet123".encode()).hexdigest()

# Fonction pour enregistrer un nouvel utilisateur
def register_user(users):
    username = input("Nom d'utilisateur : ")
    password = input("Mot de passe : ")

    hashed_password = hashlib.sha256(password.encode()).hexdigest()
    users[username] = hashed_password

    save_users(users)
    print(f"Utilisateur {username} enregistré avec succès.")

# Fonction pour authentifier un utilisateur
def authenticate_user(users):
    username = input("Nom d'utilisateur : ")
    password = input("Mot de passe : ")

    hashed_password = hashlib.sha256(password.encode()).hexdigest()

    if users.get(username) == hashed_password:
        print("Authentification réussie.")
        return True
    else:
        print("Nom d'utilisateur ou mot de passe incorrect.")
        return False

# Fonction pour ajouter un rapport
def creer_rapport():
    titre = input("Titre du rapport : ")
    contenu = input("Contenu du rapport : ")

    rapport = {
        "titre": titre,
        "contenu": contenu
    }

    with open("rapports.json", "a") as file:
        json.dump(rapport, file)
        file.write("\n")

    print("Rapport ajouté avec succès.")

# Fonction pour consulter les rapports
def consulter_rapports():
    with open("rapports.json", "r") as file:
        for line in file:
            rapport = json.loads(line)
            print(f"Titre : {rapport['titre']}")
            print(f"Contenu : {rapport['contenu']}")
            print("-" * 50)

# Fonction pour supprimer un rapport
def supprimer_rapport():
    titre = input("Titre du rapport à supprimer : ")

    with open("rapports.json", "r") as file:
        lines = file.readlines()

    with open("rapports.json", "w") as file:
        for line in lines:
            rapport = json.loads(line)
            if rapport["titre"] != titre:
                file.write(line)

    print("Rapport supprimé avec succès.")

# Fonction pour afficher le menu principal
def main_menu():
    print("\nBienvenue sur SCiPnet !")
    print("1. S'inscrire")
    print("2. Se connecter")
    print("3. Créer un rapport")
    print("4. Consulter les rapports")
    print("5. Supprimer un rapport")
    print("6. Quitter")

    choice = input("Votre choix : ")
    return choice

# Code principal
if __name__ == "__main__":
    users = load_users()
    password = load_password()

    while True:
        choice = main_menu()

        if choice == "1":
            register_user(users)
        elif choice == "2":
            if authenticate_user(users):
                print("Fonctionnalités supplémentaires à venir...")
        elif choice == "3":
            creer_rapport()
        elif choice == "4":
            consulter_rapports()
        elif choice == "5":
            supprimer_rapport()
        elif choice == "6":
            print("Au revoir !")
            break
        else:
            print("Choix invalide.")
