# Lab 1. Classical Crypto


## Done by Fedorov Alexey (<a href="https://t.me/ullibniss"><img src="https://upload.wikimedia.org/wikipedia/commons/8/83/Telegram_2019_Logo.svg" width="40"></img></a><a href="https://github.com/ullibniss/"><img src="https://user-images.githubusercontent.com/25181517/192108374-8da61ba1-99ec-41d7-80b8-fb2f7c0a4948.png" alt="drawing" width="40"></img></a>)

## Better make a review of report on [GitHub](https://github.com/ullibniss/INNO-Security-of-Systems-and-Networks/blob/master/Lab_1.md)
---

# Task 1

## 1.1. Go to https://www.dcode.fr/tools-list#cryptography or https://cryptii.com/crypto service website. Choose Vigenere ciphers and get familiar with it.

Vigenère cipher - A cipher where each letter in the plaintext is encrypted using a Caesar cipher with a different offset. The offset is determined by the position of the corresponding letter in the key within the alphabet.[[1]](https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher).

### Example
```
plaintext = "I am Fedorov Alexey bla bla bla"  
key = "aboba"

Encryption process:

plaintext:   I am Fedorov Alexey bla bla bla
key:         a bo baaboba abobaa bob aab oba
ciphertext:  I zy Eedndnv Akqwey axz blz nka

result = "I zy Eedndnv Akqwey axz blz nka"
```
## 1.2 Make a team of two, encrypt an English text of at least 50 words using the Vigenere cipher, and exchange it with your fellow student.

![image](https://github.com/user-attachments/assets/82d059fc-6032-44d7-91da-3675a352bfb3)

My team mate: **Joel Okore**  
Tool i use: **dcode.fr**

I decided to cipher famous monologue from "Pulp Fiction" said by Jules.

### Plaintext

`The path of the righteous man is beset on all sides by the inequities of the selfish and the tyranny of evil men. Blessed is he who in the name of charity and good will shepherds the weak through the valley of darkness, for he is truly his brother's keeper and the finder of lost children. And I will strike down upon thee with great vengeance and furious anger those who attempt to poison and destroy my brothers. And you will know my name is the Lord when I lay my vengeance upon thee.*`

### Key

`ABOBA`

### Result

`Tis qati cg thf fjghuspus noo is cstet pb bll twees cm uhe jbfqujhjes pt uhe tsmfitv bnd uvf tysoony pt fvim afn. Bmstsee wt he xvp in uvf nans pf ciositz ood gpce wimz theqvfrdt hie wfol thscvgh uvf vamzfy og rbrkosts, fpf ie it hsulz vjs bscuhes'g leeqss ane hie fjbeer pt mosu qiileffn. Aor J wimz ttrjyf doxb vpoo hiee xwuh gssbt vfbheaoqf ane tvripit anhss thpgf whp outendu to qcjsoo ood dfguroz az brphiert. Ood ypi xilm yoow nm oamf wt thf Zprd xvfn I moz my wsogebbde uqco thfs.`

![task-1](https://github.com/user-attachments/assets/61c965f1-9e74-424b-8d4f-1932dfd60aad)

## 1.3 Crack the encrypted text of your fellow student using the Vigenere cipher tool.

Joel sent me this cipher text:

### Ciphertext

```
Nr wvv goiwpxwwq tj wvv bdgsx,
Dzi ktsqzzuct medziq ylh srfoh tr gzmrj,
Wkoucrs oexui ms xks wcmedx pdsmw,
Ev oeqdeyx djicx phoe sqecqyji.

Rmi zwer rhtwzwvq ylucluc tsi vwetjw,
Wsczdnr xkdiq tj iciujteix wzcx,
Si zfjzrd pykx ysh gfvohs frngrc,
Giqsrhc tsi pshgsk, gwjhvne wef.

Xfj vljvf niykc s psqpdpp,
On seebk fjnrn gftolj mx llc xob,
Qrfmytrq kiawiwg, fzy ayh nwin,
Lydfuwig dsedw rmew uvbolj wvwin.

Nr wvzg rocpn gj ozmhh xfvcp,
Xsei qqszg zhn hfvbaib uefs,
Rby iy xrw rgllw’g vawrlgo, oi dnrg,
Dvoxe qsb llc wivhcsns, hexvipnrj azby.
```

I decrypted it through the Vigenère decrypt on dcode.fr. The result is:

### Key

`ALEXEYFEDOROV`

### Plaintext

```
In the stillness of the night,
The moonlight bathes the earth in white,
Shadows dance on the forest floor,
As ancient trees lean evermore.

The wind whispers through the leaves,
Telling tales of forgotten eves,
Of lovers lost and dreams undone,
Beneath the fading, distant sun.

*The river sings a lullaby,
As stars blink softly in the sky,
Carrying secrets, old and deep,
Guarding souls that gently sleep.*

In this world of quiet grace,
Time slows its hurried pace,
And in the night’s embrace, we find,
Peace for the restless, wandering mind.
```

Beautiful :)

![task-3](https://github.com/user-attachments/assets/a89792e0-640e-4f37-bbe4-1cc17bad6ff5)


## 1.4 Go through the previous two steps again, this time using a cipher of your own choosing. Do not tell your fellow student what cipher you used!

![image](https://github.com/user-attachments/assets/ed06ad89-6dbb-4e47-9e2c-7ef931a596cc)

This time i want to cipher quote from the beginning of anime "Berserk".

### Plaintext

`In this world, is the destiny of mankind controlled by some transcendental entity or law...? Is it like the hand of God hovering above? At least it is true that man has no control, even over his own will.`  

I used the cipher ROT13 because it would be funny if Joel accidentally used encryption instead of decryption and got the same result.

### Result

`Va guvf jbeyq, vf gur qrfgval bs znaxvaq pbagebyyrq ol fbzr genafpraqragny ragvgl be ynj...? Vf vg yvxr gur unaq bs Tbq ubirevat nobir? Ng yrnfg vg vf gehr gung zna unf ab pbageby, rira bire uvf bja jvyy.`

![task-4](https://github.com/user-attachments/assets/3c3a869d-2fcb-4c79-87d0-88ac9ed0c3ba)


Joel's ciphertext:

### Ciphertext

`Ihctolg fnz vkk ztdmvong rz kxxvo, tdbhor qs lwk zhvxxis ez kln rgeld.`

I ran it through a cipher analyzer. The Dcode.fr analyzer did a poor job, so I used another analyzer, Boxentriq. It identified the cipher as a Beaufort cipher. I then executed the decryptor with key lengths ranging from 1 to 15 and got a result.

### Key

`ALEXEYFEDOROV`

### Result

`Secrets are the whispers of truth hidden in the shadows of our minds`

![video-23](https://github.com/user-attachments/assets/78593a00-bc12-4cd2-93b7-ac812f8beec3)
![video-25](https://github.com/user-attachments/assets/7889b2aa-10c8-43dd-9162-36150ee62b6e)

# Task 2

## 2.1 Read about the Enigma machine, how and why it was used?

In the early 20th century, the Playfair cipher and the Jefferson cipher were used to encrypt messages. While these methods were effective, the advancement of technology created a need to mechanize the encryption process. In 1920, Dutch inventor Alexander Koch invented the first rotary cipher machine. German scientists later obtained a patent for it, made improvements, and named it Enigma.[[2]](https://habr.com/ru/articles/217331/).

Enigma is a data encryption device that was used in Germany in the early 20th century by many firms to encrypt messages. It was also used to transmit encrypted messages during the Second World War.[[2]](https://habr.com/ru/articles/217331/).

## 2.2 Explain the mechanism of its encryption/decryption.

The Enigma cipher mechanism consists of the following components[[3]](https://www.youtube.com/watch?v=ybkkiGtJmkM):

1 keyboard: Used for typing the plaintext.
1 lampboard: Used for displaying the ciphertext.
1 plugboard: Used for swapping plaintext letter inputs.
3 rings: Function as the initial vector, initializing the rotor's state.
3 rotors: Implement the cipher mechanism.
1 reflector: Closes the electrical circuit and is also part of the cipher mechanism, making decryption possible.
I would like to discuss the cipher algorithm using an example with a specific key prompt.

For example, I initialize system like this:

### Key

Rotors:**4->1->5**
Reflector: **B**
Rings: **S, U, S**

Our initial state looks like this:

![image](https://github.com/user-attachments/assets/488c1af4-0bc2-44bf-9d3c-158abe61cae9)

Let's promtp **P** letter

### Plugboard

When we press the P key, the switch completes the circuit and sends an electrical signal to the plugboard first. The plugboard is used to swap letters before they enter the cipher mechanism, adding an additional layer of complexity to the Enigma cipher.[[3]](https://www.youtube.com/watch?v=ybkkiGtJmkM)

![video-26](https://github.com/user-attachments/assets/526ca04c-d31d-431f-b6dc-6471d52ab74b)

For example we plugged in **P** and **H** together. We will have **H** on output.

### Rotors (main stage)

The signal then travels to the rotors, which function like a multi-step Caesar cipher.

The first rotor (located on the right in the machine and at the top in the picture) rotates each time a key is pressed. When it reaches a specified position, it engages with the second rotor (on the left in the machine and at the bottom in the picture), causing it to rotate as well.[[4]](https://www.codesandciphers.org.uk/enigma/rotorspec.htm)

![image](https://github.com/user-attachments/assets/b5bfffc1-3da1-4099-8d8c-ab006907bd24)

When signal reachs first rotor, it rotates for 1 position and becomes **B** position(For simple view and counting I summed **B** + **S** = **T** on picture). Now we have following picture:

![image](https://github.com/user-attachments/assets/b2d52aa9-c4c1-4fd5-895e-37d6734f8f7c)

Now there is the first transformation. Our input H undergoes a modulo 26 addition with T.

`(H + T) = A mod 26 <=> (7 + 19) = 0 mod 26`

![image](https://github.com/user-attachments/assets/a3695f05-abe6-47c5-8a6c-77277055ad45)

Rotor of type 4 has **A** - **E** letter mapping. Because of this we have letter **E** on output. 
Second rotor stays in the same position, because first rotor is type 4.

Next position counts like this:

`(Output + (Second Rotor pos - First Rotor Pos)) mod 26`

We have 

`(E + (U - T)) = F mod 26 <=> (4 + (20 - 19)) = 5 mod 26` 

![image](https://github.com/user-attachments/assets/6da9abed-be8b-4320-8b72-eb15c97578fd)

Rotor of type 1 has **F** - **G** letter mapping. Because of this we have letter **G** on output. 
Third rotor stays in the same position, because first rotor is type 1.

The last rotor can be counted the same way:

`(G + (S - U)) = E mod 26 <=> (6 + (18 - 20)) = 4 mod 26`

Letter the be sended to the reflector is:

`(G - S) = O mod 26 <=> (6 - 18) = 14 mod 26 `

![image](https://github.com/user-attachments/assets/96dd809d-941e-4c23-a79e-0d724c85cf69)

Okay, it is reflector time. Reflector maps letter in predefined way, you can see it in picture.
Letter **M** translated to 3rt rotor letter this way:

`(Reflected letter + Third rotors pos) mod 26`
`(M + S) = E mod 26 <=> (12 + 18) = 4 mod 26`

![image](https://github.com/user-attachments/assets/d251347f-ffc2-4e12-be68-4ba590c46931)

The next two rotors letters can be found the same way as before. Let's calculate.

`(X - (S - U)) = Z mod 26 <=> (23 - 18 + 20) = 25 mod 26`  
`(J - (U - T)) = I mod 26 <=> (9 - (20 - 19)) = 8 mod 26`  
`(L - T) = S mod 26 <=> (11 - 19) = 18 mod 26`

![image](https://github.com/user-attachments/assets/c9632637-5b18-4129-bb0c-a7130de1d8bb)

We have an **S** letter on output. Because we are have no plugged in **S** on plugboard, it is out letter.

## 2.3 Come with some creative text and encipher/ decipher with Enigma encryption.

![image](https://github.com/user-attachments/assets/47a941ca-9296-47d5-ae1a-e973b23ca051)


I will cipher a lines from The Doors song called "When the Music's Over".

### Plaintext

```
When the music is your special friend
Dance on fire as it intends
Music is your only friend until the end
Until the end, until the end*
```

### Key

Rotors: **3->2->2**  
Reflector: **C**  
Rings: **Z, X, C**  


### Result

`UROGFCANMAQFAGJLYTYZSKXBKVCPAJVNZWJIFSYSBXUZBULQNPUBGVTHFFSYASJLRLGHIWDLXQLAYQTEPOYSXVJUGAOJKIEKPIBWDOBPSEO`

To decrypt message, we need to repeat it with the same key.

# References

1. https://en.wikipedia.org/wiki/Vigen%C3%A8re_cipher
2. https://habr.com/ru/articles/217331/
3. https://www.youtube.com/watch?v=ybkkiGtJmkM&t=162s
4. https://www.codesandciphers.org.uk/enigma/rotorspec.htm



