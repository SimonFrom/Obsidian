- [ ] Dynamic Routing
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

- [ ] Spread
- [ ] React vs React Native
- [ ] Bundler
- [ ] Android SDK emulator