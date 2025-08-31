# 🧮 Omega Pi Stream - NumWorks Calculator App

[![Build Status](https://img.shields.io/badge/build-pending-yellow)](https://github.com/cabrerairigoyen/omega-pi-stream)

## 🚀 **Real-time LaTeX Math Display from Raspberry Pi**

Une application native pour calculatrice NumWorks (Omega firmware) qui affiche en temps réel les formules mathématiques LaTeX envoyées via UART depuis un Raspberry Pi.

### ✨ **Fonctionnalités**

- 📡 **Communication UART temps réel** avec Raspberry Pi
- 📐 **Rendu LaTeX mathématique** : `$$E=mc^2$$` → affichage visuel
- 📜 **Interface scrollable** pour flux continu
- 🎨 **Icône intégrée** dans tous les thèmes Omega
- ⚡ **Performance optimisée** pour NumWorks

### 🔧 **Architecture**

```
pi_stream_app/
├── pi_stream_app.h/.cpp      # App principale (MVC Omega)
├── pi_stream_controller.h/.cpp # Contrôleur UART + LaTeX
├── base.en.i18n              # Internationalisation
└── pi_stream_icon.png         # Icône 16x16
```

### 🛠 **APIs Utilisées**

- `Ion::Console` - Communication UART
- `Escher::ExpressionView` - Rendu LaTeX
- `Poincare::Expression` - Parsing mathématique
- `Ion::Timing` - Gestion temps réel

### 📋 **Installation**

#### **Option 1: Binpack Pré-compilé (Recommandé)**
1. Téléchargez le dernier `binpack.bin` depuis [Releases](../../releases)
2. Mode DFU sur NumWorks: RESET + 6 + USB
3. WebDFU: https://ti-planet.github.io/webdfu_numworks/n0110/
4. Flash le binpack

#### **Option 2: Integration dans Omega**
```bash
# Cloner votre fork Omega
git clone https://github.com/Omega-Numworks/Omega.git
cd Omega

# Copier l'app
cp -r /path/to/pi_stream_app ./apps/

# Ajouter à la configuration
echo "EPSILON_APPS += apps/pi_stream_app" >> build/config.mak

# Ajouter l'icône aux thèmes
cp pi_stream_app/pi_stream_icon.png themes/themes/local/*/apps/

# Build
make OMEGA_USERNAME="PiStream" binpack -j$(nproc)
```

### 🔌 **Utilisation avec Raspberry Pi**

```python
#!/usr/bin/env python3
import serial
import time

# Connexion série (adaptez le port)
ser = serial.Serial('/dev/ttyUSB0', 115200)

# Envoi formules LaTeX
formulas = [
    "$$E = mc^2$$",
    "$$\\int_0^\\infty e^{-x^2} dx = \\frac{\\sqrt{\\pi}}{2}$$",
    "$$\\sum_{n=1}^{\\infty} \\frac{1}{n^2} = \\frac{\\pi^2}{6}$$",
    "$$F = ma$$"
]

for formula in formulas:
    ser.write(formula.encode() + b'\n')
    time.sleep(2)

ser.close()
```

### 📊 **Demo Flow**

```
Raspberry Pi → [UART] → NumWorks Calculator
     ↓
"$$\sum_{n=1}^{\infty} \frac{1}{n^2} = \frac{\pi^2}{6}$$"
     ↓
Pi Stream App → LaTeX Parser → Visual Math Display
```

### 🎯 **Cas d'Usage**

- **📚 Éducation**: Formules en temps réel pendant les cours
- **🔬 Laboratoire**: Visualisation de données scientifiques  
- **🤖 Robotique**: Interface calculatrice-robot
- **📡 IoT**: Dashboard portable pour capteurs

### 🏗 **Développement**

```bash
# Structure MVC Omega
namespace PiStream {
  class App : public ::App
  class Controller : public StackViewController  
  class Snapshot : public ::App::Snapshot
}

# APIs principales
Ion::Console::readChar()     // UART non-bloquant
ExpressionView::setExpression() // Rendu LaTeX
Ion::Timing::millis()        // Polling temporel
```

### 📝 **Roadmap**

- [ ] Buffer circulaire pour historique
- [ ] Configuration vitesse UART
- [ ] Mode graphique pour plots
- [ ] Sauvegarde formules
- [ ] Interface WebSerial alternative

---

**Créé avec ❤️ pour NumWorks & Omega Community**

*Compatible: NumWorks N0110 • Omega Firmware • WebDFU Flash*
