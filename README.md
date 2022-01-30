from PIL import Image

#L'image doit être en .png car le format .jpeg a une compression différente
nom_image = "test.png"
msg = "test"

def chiffre_msg(nom_image,msg):
    #chiffrer un message

    #Lecture de l'image
    im = image.open(nom_image)
    #on récupère les dimensions de l'image
    largeur,hauteur=im.size
    #on répartis l'image en format RGB
    r,g,b = im.split()
    #on transforme l'image en liste
    r=list(r.getdata())

    #on récupère la longueur du message
    longueur = len(msg)
    #on la transforme en binaire
    binaire = bin(len(msg))[2:].rjust(8,"0")
    #on la transforme en une liste de 0 et de 1
    ascii = [bin(ord(x))[2:].rjust(8,"0") for x in msg]

    #on transforme la liste en chaine
    chaine="".join(ascii)

    #on code la longueur de la liste dans le premier octet de pixels rouges
    for j in range(8):
        r[j]=2*int(r[j]//2)+int(binaire[j])


    #on chiffre la chaine dans les pixels suivants
    for i in range (8*longueur):
        r[i+8]=2*int(r[i+8]//2)+int(chaine[i])

    #on recrée l'image rouge
    newred = image.new("L",(16*largeur,16*hauteur))
    newred = image.new("L",(largeur,hauteur))
    newred.putdata(r)

    #fusion des trois nouvelles images
    imgnew = image.merge('RGB',(newred,g,b))
    imgnew.save("resultat.png")



def get_msg(nom_couv):
    #Lire un message caché

    #Lecture de la couverture
    im = image.open(nom_couv)
    #on récupère les dimensions de l'image
    largeur,hauteur=im.size
    #on répartis l'image en format RGB
    r,g,b = im.split()
    #on transforme l'image en liste
    r=list(r.getdata())

    #lecture de la longueur de la chaine
    longueurchaine = [str(x%2) for x in r[0:8]]

    chaine="".join(longueurchaine)
    chaine=int(chaine,2)

    #lecture du message
    lecturemsg = [str(x%2) for x in r[8:8*(chaine+1)]]

    b="".join(lecturemsg)
    message=""

    for k in range(0,chaine):
        tmp=b[8*k:8*k+8]
        message = message + chr(int(tmp,2))

    print("Le message caché est :",message)


def cryptage():
    text = list("aujourd'hui est un jour particulier")
    text2 = []
    cle = list('cryptographie')
    crypt = ''
    decrypt = ''
    binaireplaintext = []
    binairekey = []
    binaireciphertext = []
    quotient = 0
    text_crypte = ''
    text_decrypte = ''
    result = ''
    temp = []
    test = []




    for b in range (len(text)):
        temp = [text[b]]
        text2 += temp
        if len(text2) % len(cle) == 0 :
            temp = []
    #print(text2)




    #            CRYPTAGE (FONCTIONNEL, VERIFIE EN EFFECTUANT LE CRYPTAGE A LA MAIN)

    text = text[:len(cle)]

    for i in range(len(text)):
        a = int(ord(text[i]))#Transformer valeur du caractère i du texte a crypter en base10
        c = int(ord(cle[i])) #Transformer valeur du caractère i de la cle en base10


        while a!=0: #Transformer valeur du caractère i du plaintext en binaire
                    quotient=a//2
                    reste=a%2
                    binaireplaintext.append(reste)
                    a=quotient
        binaireplaintext.reverse()
        while c!=0: #Transformer valeur du caractère i de la cle en binaire, puis inversion de la liste car elle est à l'envers
                    quotient=c//2
                    reste=c%2
                    binairekey.append(reste)
                    c=quotient
        binairekey.reverse()
        for t in range(len(binaireplaintext)):
                xor = binaireplaintext[t] ^ binairekey[t]
                crypt += str(xor)
        binaireplaintext.clear()
        binairekey.clear()
        val = int(crypt, 2)
        asci = chr(val % 26 + 97)
        text_crypte +=asci
        crypt = ''
    print(text_crypte)
    print(cle)



    #                           DECRYPTAGE 





    for i in range(len(text_crypte)):
        a = int(ord(text_crypte[i]))#Transformer valeur du caractère i du texte a crypter en base10
        c = int(ord(cle[i])) #Transformer valeur du caractère i de la cle en base10
        print(a)
        print(c)
        while a!=0: #Transformer valeur du caractère i du plaintext en binaire
                    quotient=a//2
                    reste=a%2
                    binaireciphertext.append(reste)
                
                    a=quotient
        binaireciphertext.reverse()
        while c!=0: #Transformer valeur du caractère i de la cle en binaire, puis inversion de la liste car elle est à l'envers
                    quotient=c//2
                    reste=c%2
                    binairekey.append(reste)
                    c=quotient
        binairekey.reverse()
        print(binairekey)
        print(binaireciphertext)
        for t in range(len(binaireciphertext)):
                xor = binairekey[t] ^ binaireciphertext[t]
                decrypt += str(xor)
        binaireciphertext.clear()
        binairekey.clear()       
        val = int(decrypt, 2)
        asci = chr(val % 26 + 97)
        text_decrypte +=asci
        decrypt = ''
    print(text_decrypte)
