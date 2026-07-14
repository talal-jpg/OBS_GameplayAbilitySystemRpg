# AtBeginPlay make an array from a seed

constexpr int32 NoiseSize = 1024;

TArray<float> NoiseArray;

void AMyActor::BeginPlay()
{
    Super::BeginPlay();

    NoiseArray.SetNum(NoiseSize);

    FRandomStream Stream(12345); // deterministic seed

    for (int32 i = 0; i < NoiseSize; i++)
    {
        NoiseArray[i] = Stream.FRandRange(-1.f, 1.f);
    }
}

# SampleNoiseAtTick

float SampleNoise(float Alpha)
{
    float Scaled = Alpha * NoiseSize;

    int32 Index0 = FMath::FloorToInt(Scaled) % NoiseSize;
    int32 Index1 = (Index0 + 1) % NoiseSize;

    float Frac = Scaled - FMath::FloorToFloat(Scaled);

    return FMath::Lerp(NoiseArray[Index0], NoiseArray[Index1], Frac);
}
