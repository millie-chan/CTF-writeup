# Krypton Level 0 → Level 1
1. when you see a string with "=" in the end, most likely base64 string. base64 decode is easy, you could even use browser console tool to decode it.  
  such as atob("S1JZUFRPTklTR1JFQVQ=")
> KRYPTONISGREAT

# Krypton Level 1 → Level 2
1. ssh krypton1@krypton.labs.overthewire.org -p 2231
1. cd /krypton/krypton1
1. ls -al
1. cat krypton2  
    > YRIRY GJB CNFFJBEQ EBGGRA  
1. it looks like substitution cipher related, can google caesar cipher or rot13  
    and bingo, it is rot13
> LEVEL TWO PASSWORD ROTTEN

# Krypton Level 2 → Level 3
1. ssh krypton2@krypton.labs.overthewire.org -p 2231
1. cd /krypton/krypton2
1. ls -al
1. cat krypton3
    > OMQEMDUEQMEK
1. ./encrypt
    > usage: encrypt foo  - where foo is the file containing the plaintext
1. cat README
    > encrypt will read /krypton/krypton2/keyfile.dat and then write a result file  
    but we dont have write permission on /krypton/krypton2  
    therefore we need to create a folder in /tmp and run command there
1. mkdir /tmp/temp20240606
1. cd /tmp/temp20240606
1. echo "ABCDEFGHIJKLMNOPQRSTUVWXYZ" > data
1. ln -s /krypton/krypton2/keyfile.dat keyfile.dat
1. /krypton/krypton2/encrypt data
1. ls -al
1. cat ciphertext
    > MNOPQRSTUVWXYZABCDEFGHIJKL
    
    A->M is 12, which means this time is rot12, to revert it we need rot(26-12)
> CAESARISEASY

# Krypton Level 3 → Level 4
1. ssh krypton3@krypton.labs.overthewire.org -p 2231
1. cd /krypton/krypton3
1. ls -al
1. cat README  
    after reading README
    1. we need to use found1, found2 and found3 to find the key
    1. and then use the key to decode krypton4
    1. there are 2 hint files but i dont want to read it
    1. if you dont want to spend time on it, you can goolge some decipher which can calculate the possible key for you
    1. such as https://www.dcode.fr/monoalphabetic-substitution and https://planetcalc.com/8047/
1. cat found1
1. cat found2
1. cat found3  
    most of them are 5 chars, only 1 with 4 chars and 1 with 2 chars, not sure if we could count on them so ignore them first  
    my plan is, list the 5-char word I know and with similar chars, and then try to match the words in those 3 found files  
    THERE, THREE, THEIR, THINK, THING, WHERE  
    first, THERE vs THREE, which are same 5 chars with different position  
    i dont want to check all of them with my own eyes, let's write a js script to do it  
    https://jsfiddle.net/5an3wuLz/2/  
    analyze "thereVsWhere UKSNS WKSNS" first, the first two chars should match one of the thinkVsThing pairs  
    but no UK or WK prefix there, which means it possibly not correct  
    analyze "thereVsTheir JDSNY JDSYS", the first two chars are JD,  
    there are two pairs of them: "thinkVsThing JDSQV JDSQE" and "thinkVsThing JDSNB JDSNY", none of them match  
    but JDE really looks like THE...at the end we still need third party tool  
1. put found1 + found2 + found3 in either website mentioned above
    got a passage with strange space
   
    > IN CRYPTOGRAPHY A CAESAR CIPHER ALSO KNOWN AS A CAESARS CIPHER  
      THE SHIFT CIPHER CAESARS CODE OR CAESAR SHIFT IS ONE OF THE SIMPLEST AND MOST WIDELY KNOWN ENCRYPTION TECHNIQUES  

    from https://en.wikipedia.org/wiki/Caesar_cipher  
    so the decryption key is BOIHGKNQVTWYURXZAJEMSLDFPC
1. cat krypton4
    > KSVVW BGSJD SVSIS VXBMN YQUUK BNWCU ANMJS  
    > WELLD ONETH ELEVE LFOUR PASSW ORDIS BRUTE  
    > well done the level four password is brute

    i did it with other's brute force XD  
        after reading the hint, what if we try https://crypto.interactive-maths.com/frequency-analysis-breaking-the-code.html  
        it's more difficult and time-consuming to solve by myself


# Krypton Level 4 → Level 5
1. ssh krypton4@krypton.labs.overthewire.org -p 2231
1. cd /krypton/krypton4
1. ls -al
1. cat README
    Vigenere Cipher  
    key length = 6  
    to be honest, without the correct place of spaces, can i really run the frequency-analysis to get the decrypted message?  
    https://www.dcode.fr/vigenere-cipher  
    decryption key is FREKEY  
1. cat krypton5
    > HCIKV RJOX  
    > CLEAR TEXT

# Krypton Level 5 → Level 6
1. ssh krypton5@krypton.labs.overthewire.org -p 2231
1. cd /krypton/krypton5
1. ls -al
1. cat README
    really no idea how to run frequency-analysis  
    https://www.dcode.fr/vigenere-cipher  
    decryption key is KEYLENGTH
1. cat krypton6
    > BELOS Z  
    > RANDO M

# Krypton Level 6 -> Level 7
1. ssh krypton6@krypton.labs.overthewire.org -p 2231
1. cd /krypton/krypton6
1. ls -al
2. cat HINT1
    > The 'random' generator has a limited number of bits, and is periodic.  
     Entropy analysis and a good look at the bytes in a hex editor will help.  
    > 
    > There is a pattern!
1. cat HINT2
    > 8 bit LFSR
1. google "8 bit LFSR"  
    found a page https://xz.aliyun.com/t/3682?time__1311=n4%2Bxnii%3DoGqqgiDRDBqDqpDUr4jxQqe7%2BPTbID  
    but still not sure what it is, no idea of cryptography
1 ./encrypt6
    > usage: encrypt6 foo bar   
    > Where: foo is the file containing the plaintext and bar is the destination ciphertext file.
1. mkdir /tmp/temp20240915
1. echo AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA > /tmp/temp20240915/plaintext1
    there are 60s A
1. /krypton/krypton6/encrypt6 /tmp/temp20240915/plaintext1 /tmp/temp20240915/ciphertext1
2. cat /tmp/temp20240915/ciphertext1
    > EICTDGYIYZKTHNSIRFXYCPFUEOCKRNEICTDGYIYZKTHNSIRFXYCPFUEOCKRN
3. the ciphertext repeated itself, first 30 chars same as last 30 chars. which means the key length should be 30?
4. echo BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB > /tmp/temp20240915/plaintext2
5. /krypton/krypton6/encrypt6 /tmp/temp20240915/plaintext2 /tmp/temp20240915/ciphertext2
6. cat /tmp/temp20240915/ciphertext2
    > FJDUEHZJZALUIOTJSGYZDQGVFPDLSOFJDUEHZJZALUIOTJSGYZDQGVFPDLSO
7. char diff was 1, so the key was always the same?
8. cat krypton7
    > PNUKLYLWRQKGKBE
9. password length is 15, then only calculate the first 15 char diff  69 73 67 84 68 71 89 73 89 90 75 84 72 78 83
    - A -> E: 4
    - A -> I: 8
    - C: 2
    - T: 19
    - D: 3
    - G: 6
    - Y: 24
    - I: 8
    - Y: 24
    - Z: 25
    - K: 10
    - T: 19
    - H: 7
    - N: 13
    - S: 18
10. try calculate and then check if the password make sense, finished the 4 chars then already know it is correct
    - P - 4 = L
    - N - 8 = F
    - U - 2 = S
    - K - 19 = R
    - L - 3 = I
    - Y - 6 = S
    - L - 24 = N
    - W - 8 = O
    - R - 24 = T
    - Q - 25 = R
    - K - 10 = A
    - G - 19 = N
    - K - 7 = D
    - B - 13 = O
    - E - 18 = M
      > LFSRISNOTRANDOM
      > LFSR IS NOT RANDOM
  11. ssh krypton7@krypton.labs.overthewire.org -p 2231, enter LFSRISNOTRANDOM, bingo
