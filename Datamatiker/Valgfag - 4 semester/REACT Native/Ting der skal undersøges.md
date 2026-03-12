- [x] Dynamic Routing
      Hvis man f.eks skal sende et id med til en anden side vil man kunne gøre det med Dynamic Routing og tilgå variablen på den nye side
- [ ] Destruct params
- [x] web/ios/android extensions til filer/Platform-Specfic code
      Man kan opdele sine komponenter eller screens med Button.web.tsx eller Button.ios.tsx. Dette gør at react selv vælger hvad der passer til den platform som der køres på.
- [x] Regions
      Fungerer ligesom i c# med 
      #region navn på region 
      ----- 
      #endregion
      Men en anden tilgang er også er opdele sine komponenter meget kraftigt, så man virkelig får lagene delt.
      
```
MyComponent/ 
- index.tsx ← eksporterer komponenten 
- MyComponent.tsx ← selve komponenten 
- useMyComponent.ts ← custom hook med logik 
- types.ts ← interfaces og types 
- styles.ts ← StyleSheet
```

- [x] Spread
      Spread er måde at tilføje ting til en liste/array. Når man spreder noget ind skaber man et nyt array med det nye objekt.
- [x] React vs React Native
      React er kun til web
- [ ] Bundler
- [x] Android SDK emulator - Er det tilsvarende at teste på en real device?
      Til vores behov er det fint med emulator. Man kan ikke teste små forskelle i hardware og andre ting. 