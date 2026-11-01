# HEARTBEAT — flotte de routines

> **Template.** Copiez en `heartbeat.md`. Décrit les routines automatiques et le dispatcher
> manuel. Dans l'architecture Porunga actuelle, les routines tournent sur le VPS via systemd;
> aucun timer Porunga n'est installé sur le Mac.

---

## Principe

Distinguez deux régimes sur le VPS :
- **Routines qui invoquent le LLM** (coûtent des tokens) → à garder rares et transparentes.
- **Services déterministes** (indexation, supervision, sauvegarde) → coûtent peu, mais ont besoin d'un verdict lisible.

> Leçon courante : une routine LLM qui tourne en arrière-plan gonfle la consommation sans que
> vous le voyiez. Préférez le **déclenchement manuel** par défaut, et n'automatisez que ce qui
> apporte un signal actionnable régulier.

## Routines (exemples)

| Routine | Cadence | Coût LLM | Sortie |
|---|---|---|---|
| `<brief matinal>` | <systemd timer VPS> | oui | <note du jour> |
| `<revue hebdo>` | <systemd timer VPS> | oui | <note hebdo> |
| `<auto-évaluation>` | <systemd timer VPS> | oui | <note eval> |
| `<surveillance prod>` | <systemd timer VPS> | oui | <alerte si 🔴 seulement> |
| `<indexation notes>` | <systemd timer VPS> | non | <index> |
| `<sauvegarde mémoire>` | <systemd timer VPS> | non | <backup vérifiable> |

## Dispatcher manuel

Un point d'entrée unique pour lancer les routines à la demande, ex. :

```bash
<assistant> jour | hebdo | eval | watchtower | ...
```

## Garde-fous

- Tout hook qui rappelle le LLM doit avoir une **garde anti-récursion**.
- Gating de cadence (ex. "ne pas relancer si < N jours depuis le dernier run").
- Les timers systemd du VPS doivent expliciter le rattrapage des runs manqués et leur verdict.
- Ne pas créer de second ordonnanceur sur un poste d'accès distant.

---

> **Comment customiser :** commencez **sans aucune routine LLM automatique**. Lancez tout à la
> main pendant quelques semaines. N'automatisez qu'une routine quand vous constatez que vous la
> lancez de toute façon tous les jours.
