Supervised Learning er når en [[ML Model]] er blevet trænet. Når dette er gjort kan man bare fodre maskinen med data og få output data lavet på ens model. 
Input data kaldes også "***Independent variable***".
Output data kaldes også "***Dependent variable***".
Tilsammen skaber de trænings dataen.
![[Pasted image 20260224095524.png]]

## Score
En test man kan lave når modellen er trænet er at kun give den Independent variablerne og se hvordan outputtet passer sammen med virkeligheden og derved give den en ***Predictive Accuracy Score***, jo højere jo bedre.

Ligningen er følgende:
$Predictive Accuacy = Correct Predections / Number of Test Instances$

![[Pasted image 20260310124513.png]]

Her vil ligningen være:
$3/4=0.75$

Her rammer vores model rigtigt 3 ud af 4 gange og er derved 75% korrekt.

### MAE (Mean Absolute Error)
En anden test man kan bruge til regression er MAE.
Her vil man kigge på differencen imellem ens resultater.
![[Pasted image 20260310124947.png]]
$MAE=Predicted+Actual/NumberOfTestInstances$

Med data fra ovenover er det som følger:
$2000+3000+2000+5000/4=3000$

Det betyder at vi kan forvente at vores models output er i gennemsnit +-3000 korrekt.

---

SL er brugbart ved når der er klare mønstre at kigge efter såsom:
- Billed filtrering
- Tekst filtrering
- SPAM filtrering

Supervised Learning laver en prediktiv model som organisere data udfra tidligere organiseret data