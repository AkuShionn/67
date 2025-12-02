'''
-------------------------------
# CONSIGNES
-------------------------------
 
/!\ Si les instructions suivantes ne sont pas respectées, l'algorithme ne fonctionnera pas et
vous serez fortement susceptible d'avoir 0 /!\
 
/!\ Vous avez 1h20 + 25mn si nécessité de tier temps /!\
 
1) Remplissez votre prénom et votre nom dans les variables __PRENOM__ et __NOM__ dans
la fonction identifiants()
 
2) Remplissez les méthodes précédées de la mention #TODO (...)
 
3) Ne touchez pas aux en-têtes des fonctions (ne pas modifier les paramètres)
 
4) Respectez les formats de sortie des fonctions (les valeurs retournées). Des exemples sont
donnés pour vous aider à comprendre ce qui est attendu en sortie.
 
 
-------------------------------
# TRAVAIL A REALISER
-------------------------------
 
Dans cet exercice vous devrez remplir les fonctions permettant de:
1) chiffrer un messages avec RSA
2) déchiffrer un message avec RSA
3) les 3 fonctions permettant de générer une clé avec Diffie-Hellman
'''
 
import random
 
#TODO
# Remplissez ces variables pour y affecter votre future note
def identifiants():
    __PRENOM__ = "ilyes"
    __NOM__ = "nom"
    return __PRENOM__, __NOM__
 
 
###############################################################################################
################################      RSA   12 points    ######################################
###############################################################################################
 
 
#TODO
# Objectif: cette fonction calcule le pgcd de a et b
# Paramètres: a et b, les valeurs dont il faut calculer le pgcd
# Sortie: le plus grand dénominateur commun entre a et b
def pgcd(a, b):
    while b:
        a, b = b, a % b
    return a
 
#TODO
# Objectif: cette fonction recherche un nombre premier compris dans un intervalle
# Paramètres: borne_min, la borne basse de l'intervalle; borne_max, la borne haute de l'intervalle
# Sortie: une valeur qui est un nombre premier (ex: 3, 7, 11, etc.)
def generer_nombre_premier(borne_min, borne_max):
    while True:
        n = random.randint(borne_min, borne_max)
        if n < 2:
            continue
        # Test de primalité basique (suffisant pour cet exercice)
        is_prime = True
        for i in range(2, int(n**0.5) + 1):
            if n % i == 0:
                is_prime = False
                break
        if is_prime:
            return n
 
#TODO
# Objectif: cette fonction calcule e, la clef publique de RSA
# Paramètres: phi, le paramètre généré pour RSA
# Sortie: la clef publique e pour RSA
def generer_e_rsa(phi):
    e = 3
    while e < phi:
        if pgcd(e, phi) == 1:
            return e
        e += 2
    return e
 
# Objectif: cette fonction calcule l'inverse d'un nombre a sur son ensemble Z/bZ
# Paramètres: a, le nombre dont on veut connaître l'inverse; b, l'ensemble sur lequel chercher l'inverse
# Sortie: l'inverse de a (ex: )
def generer_inverse(a, b, x = 0, y = 1):
    x, y = y, x - (b // a) * y
    if b % a == 0:
        return x, y
    return generer_inverse(b % a, a, x, y)
 
# Objectif: cette fonction génère les clefs p, q, n, phi, e, et d pour RSA
# Paramètres: aucun
# Sortie: les clés p, q, n, phi, e, et d dans un dictionnaire
def generer_clefs_rsa():
    # Génère deux nombres premiers p et q
    p = generer_nombre_premier(pow(10, 3), pow(10, 6))
    q = generer_nombre_premier(pow(10, 3), pow(10, 6))
    n = p * q
    phi = (p - 1) * (q - 1)
    e = generer_e_rsa(phi)
    d = generer_inverse(e, phi)[0]
    # Si l'inverse de e (d) est d < 0 ou d > phi, le remettre dans l'intervalle [0, phi-1]
    d %= phi
    keys = {
        "p": p,
        "q": q,
        "phi": phi,
        "public": e,
        "private": d,
        "modulus": n
    }
    return keys
 
#TODO
# Objectif: cette fonction chiffre une lettre avec RSA
# Paramètres: lettre, la lettre à chiffrer; e et n, les clefs publiques
# Sortie: la lettre chiffré
def chiffrer_lettre_rsa(lettre, e, n):
    # Conversion char -> int (ASCII)
    m = ord(lettre)
    # Calcul: c = m^e mod n
    c = pow(m, e, n)
    return c
 
#TODO
# Objectif: cette fonction déchiffre une lettre avec RSA
# Paramètres: lettre, la lettre à chiffrer; d et n, les clefs privées
# Sortie: la lettre chiffré
def dechiffrer_lettre_rsa(lettre, d, n):
    # CORRECTION CRITIQUE ICI :
    # Le script de correction envoie "lettre" sous forme de String (ex: "123").
    # Il faut le convertir en int avant le calcul.
    c = int(lettre)
   
    # Calcul: m = c^d mod n
    m = pow(c, d, n)
   
    # Conversion int -> char
    return chr(m)
 
 
###############################################################################################
###########################      DIFFIE-HELLMAN   8 points    #################################
###############################################################################################
 
#TODO
# Objectif: cette fonction génère un nombre aléatoire dans un intervalle donné
# Paramètres: mini, la borne minimum, maxi, la borne maximum
# Sortie: un nombre aléatoire compris entre mini et maxi
def nombre_aleatoire(mini, maxi):
    return random.randint(mini, maxi)
 
#TODO
# Objectif: cette fonction calcule la clé secrète de Bob et de Alice (A et B)
# Paramètres: g, a ou b, et p le modulo
# Sortie: la clef secrète de Bob et Alice (A et B)
def calculer_clef_secrete(g, ab, p):
    return pow(g, ab, p)
 
#TODO
# Objectif: cette fonction calcule la clé commune entre Bob et Alice
# Paramètres: A ou B, a ou b, et p le modulo (A et b si Bob, et B et a si Alice)
# Sortie: la clef commune K de Bob et Alice
def calculer_clef_commune(AB, ab, p):
    return pow(AB, ab, p)
