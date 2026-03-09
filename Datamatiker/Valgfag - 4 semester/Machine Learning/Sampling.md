For at kunne bedømme outputtet er retvisende kan vi splitte vores input data i 2 dele:
- Test data
- Training data
Tanken er at modellen man træner ikke skal have adgang til test dataen for ikke at blive biased på det. Derefter kan man så evalurere på outputtet af training dataen og se om det stemmer overens.


Der er forskellige måder at gøre det på, men sampling er en meget brugt metode.
Sampling er at man udvælger et subset fra hele ens dataset. Det oprindelige sæt kaldes "Population" og subsettet er en "Sample".

Der er flere undertyper af sampling, se evt [Geek for geeks - sampling](https://www.geeksforgeeks.org/data-science/methods-of-sampling/).

### Eksempel:
Vi har 20 studerende, 12 damer og 8 mænd. Vi skal bruge en sample på 5 studerende.
![[Pasted image 20260309102233.png]]
#### Random sampling:
Her vælger vi tilfældigt 5. Vi vælger 11, 9, 15, 19 og 3.
Disse 5 fjernes fra populationen og indgår ikke i modellens trænings data.

#### Random sampling with replacement/Bootstrapping:
Her vælger vi tilfældigt 5. Vi vælger 11, 9, 15, 19 og 15 igen.
Med replacement fjernes der ikke noget fra populationen og derved kan man vælge den samme data 2 gange. Dette gør at vores population ikke formindskes og derved kan vi få en stor training og test pool.
Dette er også godt hvis population ikke er særlig stor, da men i teorien vil have en uendelig størrelse at vælge fra.

#### Stratified random sampling:
Her inddeles populationen i underkatogorier, kaldet "Strata". Dette sikrer at fordeling af vores features i vores sample er jævn og svarere til populationen. 
Vi kan f.eks stratificere(opdele i lag, klasser eller undergrupper) på køn:
![[Pasted image 20260309104259.png]]

Nu vil man så kigge på hvad den ønskede test størrelse skal være, i vores tilfælde er det 5, altså 1/4 af den samlede population.
Det betyder så at vi tilfældigt skal vælge 1/4 fra vores 2 stratas, 3 kvinder og 2 mænd. 
![[Pasted image 20260309104610.png|386]]
