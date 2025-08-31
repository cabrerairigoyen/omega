# 🔧 Integration Guide - Pi Stream App dans Omega

## 📋 **Intégration Complète dans Omega**

### **Étape 1: Préparer Omega**
```bash
git clone https://github.com/Omega-Numworks/Omega.git
cd Omega
git submodule update --init --recursive
```

### **Étape 2: Copier l'App**
```bash
# Copier le dossier complet
cp -r pi_stream_app ./apps/

# Vérifier la structure
ls -la apps/pi_stream_app/
```

### **Étape 3: Configuration Build**
```bash
# Ajouter à la configuration
echo "EPSILON_APPS += apps/pi_stream_app" >> build/config.mak

# Ou éditer manuellement build/config.mak et ajouter:
# EPSILON_APPS += apps/pi_stream_app
```

### **Étape 4: Intégration Icônes**
```bash
# Copier l'icône dans tous les thèmes
cp pi_stream_app/pi_stream_icon.png themes/themes/local/omega_light/apps/
cp pi_stream_app/pi_stream_icon.png themes/themes/local/omega_dark/apps/
cp pi_stream_app/pi_stream_icon.png themes/themes/local/epsilon_light/apps/
cp pi_stream_app/pi_stream_icon.png themes/themes/local/epsilon_dark/apps/

# Ajouter à themes/icons.json
```

### **Étape 5: Build**
```bash
make clean
make OMEGA_USERNAME="PiStream" binpack -j$(nproc)
```

### **Étape 6: Flash WebDFU**
```bash
# Le binpack sera dans:
ls -la output/device/n0110/release/binpack.bin

# Flash via WebDFU:
# 1. NumWorks en mode DFU (RESET + 6 + USB)
# 2. https://ti-planet.github.io/webdfu_numworks/n0110/
# 3. Upload binpack.bin
```

## 🛠 **Structure Technique Complète**

### **Fichiers App**
```
apps/pi_stream_app/
├── Makefile                  # Build config
├── pi_stream_app.h          # App principale
├── pi_stream_app.cpp        # App implementation  
├── pi_stream_controller.h   # Contrôleur UART
├── pi_stream_controller.cpp # Contrôleur implementation
├── base.en.i18n            # Localisation
├── pi_stream_icon.h         # Icône header
├── pi_stream_icon.png       # Icône fichier
└── README.md               # Documentation
```

### **Modifications Omega**
```
build/config.mak             # +EPSILON_APPS += apps/pi_stream_app
themes/icons.json           # +pi_stream_icon référence
themes/*/apps/              # +pi_stream_icon.png dans chaque thème
```

### **APIs Utilisées**
```cpp
// Communication UART
#include <ion/console.h>
Ion::Console::readChar()

// Rendu LaTeX  
#include <escher/expression_view.h>
ExpressionView m_expressionView;

// Parsing mathématique
#include <poincare/expression.h>
Poincare::Expression::parse()

// Timing
#include <ion/timing.h>
Ion::Timing::millis()
```

## 🎯 **Vérification Installation**

### **Build Success**
```bash
# Build réussi si:
ls -la output/device/n0110/release/binpack.bin
# Fichier ~2-3MB

# Logs build normaux:
# - Compiling apps/pi_stream_app/...
# - Linking...
# - Creating binpack.bin
```

### **Flash Success**
```bash
# Sur NumWorks après flash:
# 1. Menu principal → Nouvelles apps
# 2. Icône "🧮" Pi Stream visible
# 3. App démarre sans erreur
# 4. Interface UART affichée
```

### **Test UART**
```python
# Test Raspberry Pi
import serial
ser = serial.Serial('/dev/ttyUSB0', 115200)  
ser.write(b'Test: $$x^2 + y^2 = r^2$$\n')
# Doit apparaître sur NumWorks
```

## 🐛 **Troubleshooting**

### **Erreur Build**
```bash
# Si erreur "app not found":
ls -la apps/pi_stream_app/Makefile  # Doit exister

# Si erreur "icon not found":  
ls -la themes/*/apps/pi_stream_icon.png  # Dans tous thèmes

# Si erreur linking:
grep "pi_stream_app" build/config.mak  # Doit être présent
```

### **Erreur Flash**
```bash
# Si DFU non détecté:
# - Maintenir RESET + 6 + USB plus longtemps
# - Essayer autre port USB
# - Vérifier WebDFU compatible

# Si flash échoue:
# - Vérifier taille binpack < 4MB
# - Réessayer avec nouveau binpack
```

### **App non visible**
```bash
# Si app n'apparaît pas:
# - Vérifier icône dans thème actuel
# - Redémarrer NumWorks (éteindre/rallumer)  
# - Vérifier version Omega compatible
```

---

**✅ Une fois intégré, votre app Pi Stream sera native dans Omega avec toutes les fonctionnalités!**
