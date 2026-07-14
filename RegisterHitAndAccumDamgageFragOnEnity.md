
void AMyProjectile::RegisterMassHit(
	FMassEntityHandle Entity,
	const FHitResult& Hit)
{
	UMassEntitySubsystem* MassSubsystem =
		GetWorld()->GetSubsystem<UMassEntitySubsystem>();

	if (!MassSubsystem) return;

	FMassEntityManager& EntityManager =
		MassSubsystem->GetMutableEntityManager();

	FMassCommandBuffer& Cmd = EntityManager.Defer();

	Cmd.AddFragment<FPendingDamageFragment>(Entity);
	Cmd.AddFragment<FHitInfoFragment>(Entity);

	const float Damage = 25.f;

	Cmd.PushCommand<FMassDeferredSetCommand>(
		Entity,
		[Damage, Hit](FMassEntityView View)
		{
			View.GetFragmentData<FPendingDamageFragment>().Damage += Damage;

			auto& HitInfo = View.GetFragmentData<FHitInfoFragment>();
			HitInfo.HitLocation = Hit.ImpactPoint;
			HitInfo.HitNormal   = Hit.ImpactNormal;
			HitInfo.RawDamage   = Damage;
		});
}
