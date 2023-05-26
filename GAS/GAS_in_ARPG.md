# GAS in ARPG sample


[Document](https://docs.unrealengine.com/4.27/en-US/Resources/SampleGames/ARPG/GameplayAbilitiesinActionRPG/)

## 调用顺序

ARPGCharacterBase: TryActivateAbility ->

Melee:

- GA_MeleeBase: CommitAbility, Play Montage, then wait for event ->
- MontageEvent: notify WeaponActor and start checking collision ->
- WeaponActor: ActorBeginOverlap will call SendGameplayEventToActor ->
- GA_MeleeBase: reveive event, do corresponding effect ->
- GE_MeleeBase: calculate damage value, then set Attribute(Damage/HP) in PostGameplayEffectExecute

Range:  

- GA_SpawnProjectileBase: CommitAbility, Play Montage, then wait for event ->
- MontageEvent: SendGameplayEventToActor ->
- GA_SpawnProjectileBase: reveive event, spawn projectile actor ->
- BP_AbilityProjectileBase: ActorBeginOverlap will call ApplyGameplayEffectSpec ->
- GE: same as Melee
