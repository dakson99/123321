# 123321
123321
/*RSA*/
import string
alphabet = string.ascii_lowercase

moje_ime = 'Mladen'
mojeIme = (''.join(char for char in moje_ime if char.isalnum())).lower()
print('Moje ime je:\t\t\t\t', mojeIme)
num_ekvivalent = 0
for char in mojeIme:
if char.isalpha(): 
num_ekvivalent += alphabet.index(char)
Q = num_ekvivalent
print('Numerički ekvivalent imena je:\t\t', Q)
N = 100 + Q % 26
Z = pow(2,N)  
import gmpy2
p = Z-1
q = Z+1
while (gmpy2.is_prime(p) != True):
    p -= 2
while (gmpy2.is_prime(q) != True):
    q += 2

print('Za p je dobijena vrednost:\t\t', p)
print('Za q je dobijena vrednost:\t\t', q)

n = p * q
print('Vrednost za modul n je:\t\t\t', n)
phi = (p-1)*(q-1)
e=65537    
d = gmpy2.invert(e,phi) 
print('Privatan ključ d je:\t\t\t', d)
inputValue = Q
cipiheredValue = gmpy2.powmod(inputValue,e,n)
print('Šifrat za vrednost Q =', inputValue, ' je:\t\t', cipiheredValue)
decipiheredValue = gmpy2.powmod(cipiheredValue,d,n)
print('Dešifrovana vrednost za Q =', inputValue, ' je:\t', decipiheredValue)



/*AES*/
from cryptography.hazmat.primitives import hashes                   
from cryptography.hazmat.primitives.kdf.pbkdf2 import PBKDF2HMAC   
from cryptography.hazmat.primitives.kdf.scrypt import Scrypt 
from cryptography.hazmat.backends import default_backend
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes            
backend = default_backend()
import os


#salt= os.urandom(16)
salt = b'\x0b\xe8\xe85\x91\xda\x88H\xb6"\xafsN8Y\xf5'
print('Salt vrednost je:\t\t', salt)

kdf = Scrypt(salt = salt, length = 32,
                    n = 2**14, 
                    r=8, 
                    p=1,
                    backend = default_backend())

key = kdf.derive(b'Mladen920/2017')

kdf = PBKDF2HMAC(algorithm=hashes.SHA256(),
                    length = 16, 
                    salt = salt,                     
                    iterations = 100000,
                    backend=backend)

iv = kdf.derive(b'Janjic920/2017')

aesInstance = Cipher(algorithms.AES(key),        
                   modes.CBC(iv),               
                   backend=default_backend())

aesEncryptor = aesInstance.encryptor()
aesDecryptor = aesInstance.decryptor()

message = b'Danas polazemo Zastitu podataka u 13:00h'
print('Otvoreni tekst poruke je:\t', message)

message += b" " * (-len(message) % 16)

encryptedText = aesEncryptor.update(message)
print('Šifrat poruke je:\t\t', encryptedText)


decryptedText = aesDecryptor.update(encryptedText)
print('Dešifrovana poruka je:\t\t', decryptedText)

