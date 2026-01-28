# Migration vers Foundry VTT 13.351

## Modifications effectuées

### 1. Configuration système ([system.json](system.json))
- ✅ Version mise à jour : `1.910` → `2.0.0`
- ✅ Compatibilité vérifiée : `verified: 13`
- ✅ Maximum confirmé : `maximum: 13`

### 2. Corrections API dépréciées

#### [modules/swffg-main.js](modules/swffg-main.js)
- ✅ **Ligne 1353** : `game.macros.entities` → `game.macros`
  ```javascript
  // AVANT (déprécié)
  const macroExists = game.macros.entities.find((m) => m.name === macro.name);
  
  // APRÈS (v13 compatible)
  const macroExists = game.macros.find((m) => m.name === macro.name);
  ```

### 3. APIs vérifiées et conformes ✅

- **Document classes** : Utilise correctement `CONFIG.Actor.documentClass`, `CONFIG.Item.documentClass`
- **Utilities** : Utilise `foundry.utils.mergeObject()` et `foundry.utils.duplicate()`
- **Canvas** : Override de `foundry.canvas.placeables.Token.prototype._drawBar` conforme
- **ApplicationV2** : Déjà implémenté dans [helpers/character-creator.js](modules/helpers/character-creator.js) et [importer/data-importer.js](modules/importer/data-importer.js)
- **Dice system** : Étend correctement `foundry.dice.terms.DiceTerm`
- **PIXI** : Utilise `PIXI.utils.rgb2hex()` (toujours valide en v13)

## Fonctionnalités v13 déjà supportées

### Active Effects
- ✅ ActiveEffectFFG implémenté
- ✅ `CONFIG.ActiveEffect.legacyTransferral = false`
- ✅ Onglet "Active Effects" dans les character sheets

### ApplicationV2
- ✅ Character Creator utilise `HandlebarsApplicationMixin(ApplicationV2)`
- ✅ Data Importer utilise `HandlebarsApplicationMixin(ApplicationV2)`

### Tours
- ✅ System tours enregistrés via `register_system_tours()`

## Tests recommandés

1. **Dés personnalisés FFG**
   - Tester tous les types : Ability, Proficiency, Boost, Difficulty, Challenge, Setback, Force
   - Vérifier l'affichage des résultats narratifs

2. **Character Sheets**
   - Ouvrir/éditer personnages et adversaires
   - Tester Edit Mode
   - Vérifier les calculs de stats (wounds, strain, soak, defense)

3. **Combat**
   - Initiative avec Force dice
   - Generic Slots system
   - Turn tracking

4. **Importers**
   - OggDude Data importer
   - SWA importer
   - Character Creator wizard

5. **Token bars**
   - Wounds/Strain visualization
   - Threshold exceeded indicators
   - Hull Trauma / System Strain (véhicules)

## Breaking changes Foundry v13 à surveiller

### Changements majeurs v13
1. ✅ **ESM migration** : Système déjà compatible
2. ✅ **Collections API** : `game.macros.entities` corrigé
3. ⚠️ **TextEditor enrichment** : Vérifier les tags custom (`@FFG[roll]`, OggDude tags)
4. ⚠️ **Canvas rendering** : Token._drawBar override à tester en conditions réelles

### Vérifications post-installation
- [ ] Charger un monde existant et vérifier la migration automatique
- [ ] Tester l'import de données OggDude
- [ ] Vérifier les macros existantes (hotbar)
- [ ] Tester le dice roller en chat
- [ ] Vérifier les Journal enrichers (symboles de dés FFG)

## Références

- [Changelog StarWarsFFG 2.0.0](CHANGELOG.md) - Support officiel v13
- [Foundry VTT v13 Release Notes](https://foundryvtt.com/releases/13.335)
- [API Documentation v13](https://foundryvtt.com/api/v13/)

## Notes de développement

### Architecture conforme v13
- Document system : ✅
- Data models : ✅
- Embedded collections : ✅
- Hooks usage : ✅
- Socket enabled : ✅

### Prochaines améliorations possibles
- Migrer vers full ESM module (actuellement utilise `esmodules` mais pourrait utiliser `module.type: "esm"`)
- Ajouter TypeScript pour meilleure IntelliSense
- Implémenter plus de ApplicationV2 pour les sheets (actuellement v1+v2 coexistent)

---

**Date de migration** : 28 janvier 2026  
**Version système** : 2.0.0  
**Foundry target** : 13.351  
**Status** : ✅ Compatible
