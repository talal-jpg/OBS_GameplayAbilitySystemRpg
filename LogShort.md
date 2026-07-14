[2026.07.14-04.00.03:275][  0]LogConsoleManager: Warning: Console variable 'r.MotionVectorSimulation' used in the render thread. Rendering artifacts could happen. Use ECVF_RenderThreadSafe or don't use in render thread.
[2026.07.14-04.00.03:413][  0]LogSlate: Slate font cache was flushed
[2026.07.14-04.00.03:437][  0]LogVulkanRHI: Creating new VK swapchain with VK_PRESENT_MODE_IMMEDIATE_KHR, VK_FORMAT_B8G8R8A8_UNORM, VK_COLOR_SPACE_SRGB_NONLINEAR_KHR, num images 3
[2026.07.14-04.00.03:443][  0]LogSlate: InvalidateAllWidgets triggered.  All widgets were invalidated
[2026.07.14-04.00.03:443][  0]LogTrace: Display: Control listening on port 1985
[2026.07.14-04.00.03:448][  1]LogRenderer: Forcing update for all mesh draw commands: SkyLight real-time capture change
[2026.07.14-04.00.03:449][  1]LogRenderer: Warning: Screen Space Global Illumination (SSGI) is deprecated (from 5.8) and will be removed in a future release. Please migrate to Lumen Global Illumination.
[2026.07.14-04.00.03:449][  1]LogSlate: Display: Synchronously lazy loading font file '../../../Engine/Content/Slate/Fonts/Roboto-Regular.ttf' with subface index 0 in Game thread.
[2026.07.14-04.00.03:449][  1]LogSlate: Display: Took 0.000257 seconds to synchronously load lazily loaded font '../../../Engine/Content/Slate/Fonts/Roboto-Regular.ttf' (155K) on Game thread.
[2026.07.14-04.00.03:449][  1]LogSlate: Display: Freetype font face in memory successfully created with synchronously loaded raw font file '../../../Engine/Content/Slate/Fonts/Roboto-Regular.ttf' subface 0 in Game thread.
[2026.07.14-04.00.03:449][  1]LogSlate: Display: Font face '../../../Engine/Content/Slate/Fonts/Roboto-Regular.ttf' subface index 0 successfully added to font face map in Game thread.
[2026.07.14-04.00.03:451][  1]LogSlate: Display: Synchronously lazy loading font file '../../../Engine/Content/EngineFonts/Faces/RobotoBold.ufont' with subface index 0 in Game thread.
[2026.07.14-04.00.03:451][  1]LogSlate: Display: Took 0.000238 seconds to synchronously load lazily loaded font '../../../Engine/Content/EngineFonts/Faces/RobotoBold.ufont' (160K) on Game thread.
[2026.07.14-04.00.03:451][  1]LogSlate: Display: Freetype font face in memory successfully created with synchronously loaded cooked font file '../../../Engine/Content/EngineFonts/Faces/RobotoBold.ufont' subface 0 in Game thread.
[2026.07.14-04.00.03:451][  1]LogSlate: Display: Font face '../../../Engine/Content/EngineFonts/Faces/RobotoBold.ufont' subface index 0 successfully added to font face map in Game thread.
[2026.07.14-04.00.05:936][420]LogOnlineSession: STEAM: Found 1 lobbies, finalizing the search
[2026.07.14-04.00.05:936][420]LogOnlineSession: STEAM: Search result 0: LobbyId=Lobby[0x1860000804FEE60], LobbyId.IsValid()=true, CSteamID(LobbyId).IsLobby()=true
[2026.07.14-04.00.05:937][420]LogBlueprintUserMessages: [MM_MainMenu] Found GameName: AuraRedo3
[2026.07.14-04.00.06:226][453]LogOnlineSession: STEAM: Using P2P Data for Connection Serialization
[2026.07.14-04.00.06:227][454]LogGameMode: Display: Match State Changed from InProgress to LeavingMap
[2026.07.14-04.00.06:227][454]LogGameState: Match State Changed from InProgress to LeavingMap
[2026.07.14-04.00.06:227][454]LogGlobalStatus: Browse Started Browse: "steam.76561198064996934/Game/Maps/MM_MainMenu"
[2026.07.14-04.00.06:227][454]LogNet: Browse: steam.76561198064996934/Game/Maps/MM_MainMenu
[2026.07.14-04.00.06:229][454]LogCsvProfiler: Display: Metadata set : iris="0"
[2026.07.14-04.00.06:229][454]LogNet: InitBase PendingNetDriver (NetDriverDefinition GameNetDriver) using replication model Generic
[2026.07.14-04.00.06:230][454]LogInit: BSD IPv4/6: Socket queue. Rx: 425984 (config 262144) Tx: 65536 (config 32768)
[2026.07.14-04.00.06:230][454]LogNet: Created socket for bind address: 0.0.0.0:0
[2026.07.14-04.00.06:230][454]LogNet: IpConnection_2147482313 setting maximum channels to: 32767
[2026.07.14-04.00.06:230][454]PacketHandlerLog: Loaded PacketHandler component: Engine.EngineHandlerComponentFactory (StatelessConnectHandlerComponent)
[2026.07.14-04.00.06:240][454]LogHandshake: Stateless Handshake: NetDriverDefinition 'GameNetDriver' CachedClientID: 1
[2026.07.14-04.00.06:240][454]LogNet: AddressResolution: Begin [Host=steam.76561198064996934] [Port=7777] [Type=Hostname]
[2026.07.14-04.00.06:241][454]LogNet: Game client on port 7777, rate 100000
[2026.07.14-04.00.06:241][454]LogNetVersion: Aura 1.0.0.0, NetCL: 55116800, EngineNetworkVersion: 44, GameNetworkVersion: 0 (Checksum: 3723558420)
[2026.07.14-04.00.06:287][459]LogSockets: Warning: GetAddressInfo failed to resolve host with error SE_HOST_NOT_FOUND [-2]
[2026.07.14-04.00.06:287][459]LogNet: Warning: AddressResolution: Result [Host=steam.76561198064996934] [FAILED] [Error=48]
[2026.07.14-04.00.06:287][459]LogNet: UNetConnection::Close: [UNetConnection] RemoteAddr: , Name: IpConnection_2147482313, Driver: Name:PendingNetDriver Def:GameNetDriver IpNetDriver_2147482376, IsServer: NO, PC: NULL, Owner: NULL, UniqueId: INVALID, Channels: 3, Time: 2026.07.14-04.00.06
[2026.07.14-04.00.06:287][459]LogNet: UNetConnection::SendCloseReason:
[2026.07.14-04.00.06:287][459]LogNet:  - Result=AddressResolutionFailed, ErrorContext="AddressResolutionFailed"
[2026.07.14-04.00.06:287][459]LogNet: UChannel::Close: Sending CloseBunch. ChIndex == 0. Name: [UChannel] ChIndex: 0, Closing: 0 [UNetConnection] RemoteAddr: , Name: IpConnection_2147482313, Driver: Name:PendingNetDriver Def:GameNetDriver IpNetDriver_2147482376, IsServer: NO, PC: NULL, Owner: NULL, UniqueId: INVALID
[2026.07.14-04.00.06:293][460]LogNet: Error: UEngine::BroadcastNetworkFailure: FailureType = PendingConnectionFailure, ErrorString = Your connection to the host has been lost., Driver = Name:PendingNetDriver Def:GameNetDriver IpNetDriver_2147482376
[2026.07.14-04.00.06:293][460]LogNet: Warning: Network Failure: PendingNetDriver[PendingConnectionFailure]: Your connection to the host has been lost.
[2026.07.14-04.00.06:293][460]LogNet: NetworkFailure: PendingConnectionFailure, Error: 'Your connection to the host has been lost.'
[2026.07.14-04.00.06:293][460]LogGameMode: Display: Match State Changed from LeavingMap to Aborted
[2026.07.14-04.00.06:293][460]LogGameState: Match State Changed from LeavingMap to Aborted
[2026.07.14-04.00.06:293][460]LogNet: DestroyNamedNetDriver: Name:PendingNetDriver Def:GameNetDriver IpNetDriver_2147482376 
[2026.07.14-04.00.06:306][461]LogGameMode: Display: Match State Changed from Aborted to LeavingMap
[2026.07.14-04.00.06:306][461]LogGameState: Match State Changed from Aborted to LeavingMap
[2026.07.14-04.00.06:306][461]LogGlobalStatus: Browse Started Browse: "/Game/Maps/MM_MainMenu?closed"
