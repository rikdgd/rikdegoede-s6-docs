---
sidebar_position: 1
---
# Project Proposal
# Secure file storage (LockBox)

Voor mijn individuele project wil ik graag een applicatie ontwikkelen voor het opslaan van bestanden. De applicatie zal vergelijkbaar zijn met bestaande applicaties zoals **Google Drive** en **Dropbox**. Het idee is dat gebruikers een account kunnen aanmaken, en vervolgens bestanden kunnen uploaden naar de applicatie.


### Technische uitdagingen:
- Bestanden moeten worden versleuteld met een veilig algoritme zoals AES-GCM.
- Het systeem moet beschermd zijn tegen malafide bestanden.
- Gebruikersaccounts moeten goed beveiligd zijn om opgeslagen bestanden te beschermen.
- Gebruikers moeten bestanden kunnen delen via tijdelijke URL's.
- De bestanden moeten efficiënt worden opgeslagen om zo veel mogelijk opslagruimte te besparen.


### Mogelijke services:
Het is moeilijk om vooraf precies te bepalen welke verschillende services de applicatie nodig zal hebben. Hier is toch een poging gedaan om een aantal services te bedenken die de applicatie nodig zal hebben.

- **File storage service** - Er zal een service nodig zijn voor het opslaan van bestanden, dit is namelijk de hoofdfunctionaliteit van de applicatie.
- **User service** - Aangezien de applicatie gebruikersaccounts bevat, is het waarschijnlijk  handig om een service te maken voor het beheren van accounts. Zo kan de gebruiker bijvoorbeeld zijn/haar eigen account aanmaken of verwijderen.
- **File sharing service** - De applicatie moet in staat zijn om bestanden te delen via een tijdelijke download URL. Dit kan mogelijk beter worden toegevoegd aan de *File storage service*, maar mogelijk is het beter om dit op te splitsen.
- **File scanning service** - De bestanden die gebruikers uploaden kunnen mogelijk malware bevatten. Om te voorkomen dat gebruikers deze bestanden uploaden kan een file scanning service worden gebruikt om alle geüploade bestanden te controleren. 
