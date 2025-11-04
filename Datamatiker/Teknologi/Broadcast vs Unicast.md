##### Unicast:

1. I et klassisk Unicast scenarie vil der være flere forskellige pc'er kablet til en hub.
2. En frame bliver sendt med den ønskede modtagers MAC adresse til hubben som kopierer og sender videre ud til at alle andre pc'ers netværks kort.
3. Hver enkelt netværks kort tjekker MAC adressen på framens modtager del og sammenligner med sin egen.
4. Matcher den senders dataen videre op i softwaren.
5. Matcher den ikke, destrueres og glemmes den.
 
Broadcast:  
I et broadcast scenarie vil den indkomne frame have en modtager MAC adresse som hedder "FF-FF-FF-FF-FF-FF", som betyder at alle pc'er der modtager denne frame skal skubbe dataen videre i systemet.
 
Når flere pc'er er koblet på et kablet netværk og er synlige for hinanden indgår de i et "Broadcast Domain".