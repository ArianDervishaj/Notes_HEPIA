Shannon :
$C = Bp\cdot lb(1 + SNR)$
$\dot{M}$ : débit du moment
$M = 2 \cdot Bp$ (théorique) \[Symbole/s]
$\dot{D} = \dot{M} \cdot lb\left(\text{valence}\right)$ 
valence = nbr de niveaux.
Wifi 7: $320MHz -> \dot{M} = 2 * 320 = 640$ MBd
4096 valence -> lb(4096) = 12
$\dot{D} = 12 * 640 = 7.680\ Gbit/s$ 
Bd != bit/s mais ils sont en égale lorsque les symboles = 1 bit.

# Canaux d'accès Wifi
## Ondes
Wifi pas fait pour la mobilité
802.11 -> Wifi
802.16 -> WiMAX

controlleur donne des access points

radius = serveur AAA  (Auth Accounting, authorisation)

étallement de spectre

![[WiFi et WiMAx 2024-10-14 17.35.51.excalidraw]]

PSK : phase shift keying
PAM  phase amplitude modulation

![[Pasted image 20241014173122.png]]

# Les réseaux sans fils

## Modulation
DSSS (Direct Sequence Spread Spectrum, étalement de spectre à sàquence directe)

| Technologie | Codage                        | Type de modulation | Débit   |
|-------------|-------------------------------|--------------------|---------|
| 802.11b     | 11 bits (Barker sequence)     | PSK                | 1Mbps   |
| 802.11b     | 11 bits (Barker sequence)     | QPSK               | 2Mbps   |
| 802.11b     | CCK (4 bits)                  | QPSK               | 5.5Mbps |
| 802.11b     | CCK (8 bits)                  | QPSK               | 11Mbps  |
| 802.11a     | CCK (8 bits)                  | OFDM               | 54Mbps  |
| 802.11g     | CCK (8 bits)                  | OFDM               | 54Mbps  |

![[Pasted image 20241014174103.png]]

Les boucles sont utiles car si la fibre est coupé permet quand meme communiquer le temps de ressouder 

On peut remplacer la fibre par du sans fil

# Mats d'antenne
![[Pasted image 20241014174900.png]]
# les réseaux sans fils Transmission 1
![[Pasted image 20241014174939.png]]
![[Pasted image 20241014174953.png]]

$\lambda =$ longueur d'onde du rayonnement
$S =$ surface du paraboloide
$k =$ coefficient d'efficacité 

$\rho = \frac{1}{2}\sqrt{D\cdot \lambda}$ 
$R'= R \frac{4}{3}= 8500$km 
$y = \frac{D^{2}}{8R'}$

![[Pasted image 20241014175845.png]]

# Bande pour WiMax
![[Pasted image 20241014175838.png]]

![[Pasted image 20241014180440.png]]

![[Pasted image 20241014180446.png]]

![[Pasted image 20241014180457.png]]