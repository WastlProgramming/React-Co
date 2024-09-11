Vitest ⚡
===========================================================


Einführung
-------------

Diese Dokumentation beschreibt, wie man **Vitest** zusammen mit **React Hook Form** einsetzt, um Unit-Tests für eine Formular-Komponente in einer React-Anwendung zu schreiben. Wir gehen detailliert auf die verschiedenen Tests ein, erklären, wann und warum bestimmte Methoden verwendet werden und wie man auf verschiedene Elemente der Komponente zugreifen kann.

Wichtige Konzepte
~~~~~~~~~~~~~~~~~~~~~~~~~

Bevor wir tiefer einsteigen, sollten einige grundlegende Konzepte geklärt werden:

- **Vitest**: Ein schnelles Unit-Test-Framework, das speziell für moderne JavaScript-Frameworks wie React entwickelt wurde.
- **React Hook Form**: Ein beliebtes Formular-Handling-Paket für React, das es ermöglicht, Formulare einfach zu erstellen und zu validieren.
- **TestWrapper**: Eine Hilfskomponente, die React Hook Form in unseren Tests bereitstellt.

.. important::

   Die richtige Einrichtung und Nutzung von Hilfskomponenten wie `TestWrapper` ist entscheidend für das Testen von Formularen, da sie sicherstellt, dass alle Hook-Formulare korrekt gemanaged werden. 🎯

Vorgehen ⚙️
-------------------------

Beim Testen wird nach dem 3-Schritte-Prinzip vorgegangen 🚶

1. **Arrange**
   Vorbereitung und Initialisierung der notwendigen Voraussetzungen für den Test:
   
   - Testdaten oder Objekte erstellen
   - Testumgebung konfigurieren
   - Externe Abhängigkeiten (Mocks, Stubs, etc.) vorbereiten

   .. code-block:: js

      // Beispiel:
      const calculator = new Calculator();

2. **Act**
   Ausführung der zu testenden Aktion:

   - Die spezifische Methode oder Funktion aufrufen
   - Interaktionen simulieren

   .. code-block:: js

      // Beispiel:
      const result = calculator.add(5, 10);

3. **Assert**
   Überprüfung, ob das erwartete Ergebnis mit dem tatsächlichen Ergebnis übereinstimmt:

   - Ergebnisse validieren
   - Sicherstellen, dass der Test erfolgreich verläuft oder fehlgeschlagen ist

   .. code-block:: js

      // Beispiel:
      expect(result).toBe(15);

Wichtige Gets 🕵️‍♂️
------------------------------------

Beim Testen von UI-Komponenten gibt es verschiedene Möglichkeiten, um Elemente zu selektieren. Hier sind einige der wichtigsten Methoden, die häufig verwendet werden:

1. **getByRole**
   Sucht nach einem Element basierend auf seiner Rolle (z.B. Button, Checkbox, etc.).
   
   - Verwendung, wenn du nach spezifischen semantischen Rollen suchst.
   
   .. code-block:: js

      // Beispiel: Suche nach einem Button-Element
      const submitButton = screen.getByRole('button', { name: /submit/i });

2. **getByText**
   Sucht nach einem Element, das einen bestimmten Text enthält.
   
   - Besonders nützlich, um Elemente anhand ihres sichtbaren Textinhalts zu finden.
   
   .. code-block:: js

      // Beispiel: Suche nach einem Element mit Text "Login"
      const loginLink = screen.getByText('Login');

3. **getByLabelText**
   Sucht nach einem Formular-Element basierend auf seinem zugehörigen Label.
   
   - Optimal für Formularfelder wie Input, Textareas, etc.
   
   .. code-block:: js

      // Beispiel: Suche nach einem Eingabefeld mit Label "Username"
      const usernameInput = screen.getByLabelText('Username');

4. **getByPlaceholderText**
   Sucht nach einem Input-Element, das den angegebenen Platzhaltertext enthält.
   
   - Hilfreich, wenn Labels nicht verfügbar sind, aber Platzhalter verwendet werden.
   
   .. code-block:: js

      // Beispiel: Suche nach einem Input-Element mit Platzhalter "Passwort eingeben"
      const passwordInput = screen.getByPlaceholderText('Passwort eingeben');

5. **getByTestId**
   Sucht nach einem Element, das ein spezifisches `data-testid` Attribut besitzt.
   
   - Gut geeignet, um spezifische Elemente zu testen, die keine klaren Labels oder Texte haben.
   
   .. code-block:: js

      // Beispiel: Suche nach einem Element mit Test-ID "submit-button"
      const submitButton = screen.getByTestId('submit-button');


Queries
~~~~~~~~~~~~~~~~~~~~~

.. image:: /_static/queries.png

Tests im Detail
------------------

1. **Test: Überschrift wird korrekt gerendert** 💡
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Der erste Test überprüft, ob die Überschrift "Kundendaten" korrekt angezeigt wird. Hier ein Beispiel, wie man die Komponente rendert und dann die Überschrift im DOM sucht:

.. code-block:: react

    describe('CustomerData.tsx', () => {
      test('renders subheading "Kundendaten"', () => {
        render(
          <TestWrapper>
            <CustomerData />
          </TestWrapper>
        );
        const headingElement = screen.getByText(/Kundendaten/i);
        expect(headingElement).toBeInTheDocument();
      });
    });

.. tip::

   - **screen.getByText()**: Diese Methode durchsucht den DOM nach einem Textinhalt, der dem übergebenen Wert entspricht. Hier wird nach "Kundendaten" gesucht.
   - **expect(...).toBeInTheDocument()**: Überprüft, ob das Element wirklich im DOM existiert.

Wenn du eine Überschrift oder einen bestimmten Text in deiner Komponente testen möchtest, ist dies die einfachste und präziseste Methode. 🎯

2. **Test: Alle Eingabefelder werden korrekt gerendert** 📋
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dieser Test überprüft, ob alle erwarteten Eingabefelder (z.B. Kundennummer, Vorname, Nachname) vorhanden sind. Es wird eine Liste von Feldbezeichnern durchsucht und jedes dieser Felder im DOM überprüft:

.. code-block:: react

    test('renders all input fields', () => {
      render(
        <TestWrapper>
          <CustomerData />
        </TestWrapper>
      );

      const fields = [
        { label: /Kundennummer/i },
        { label: /Vorname/i },
        { label: /Nachname/i },
        { label: /Geburtsdatum/i },
        { label: /Straße und Hausnummer/i },
        { label: /PLZ/i },
        { label: /Stadt/i },
        { label: /Land/i },
        { label: /Telefon/i },
        { label: /E-Mail Adresse/i },
      ];

      fields.forEach(({ label }) => {
        const field = screen.getByLabelText(label);
        expect(field).toBeInTheDocument();
      });
    });

.. tip::

   - **screen.getByLabelText()**: Hier wird nach Eingabefeldern gesucht, die mit einem bestimmten Label (z.B. "Vorname") assoziiert sind.
   - Die Verwendung von Regex (`/Vorname/i`) macht die Suche nach Labels flexibler, da Groß- und Kleinschreibung ignoriert werden.

Dieser Test stellt sicher, dass das Formular alle erforderlichen Felder korrekt rendert, was für die Benutzerfreundlichkeit des Formulars entscheidend ist. 📄

3. **Test: Hilfetext für Kundennummer** 📝
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Der dritte Test prüft, ob der richtige Hilfetext für das Feld "Kundennummer" angezeigt wird, falls der Benutzer noch keine Kundennummer besitzt. Dieser Text informiert den Benutzer, was zu tun ist, wenn er keine gültige Nummer hat.

.. code-block:: React

    test('testing for customerNumber helperText', () => {
      render(
        <TestWrapper>
          <CustomerData />
        </TestWrapper>
      );
      const customerNumberHelperText = screen.getByText(
        /Falls keine vorhanden, „NEU“ eingeben. Bitte nachreichen./i
      );
      expect(customerNumberHelperText).toBeInTheDocument();
    });

.. note::

   - **screen.getByText()**: Hier wird erneut nach einem Textinhalt gesucht, in diesem Fall der Hinweistext für das Kundennummernfeld.
   - Hilfetexte sind wichtig, um Benutzer bei der Eingabe ihrer Daten zu unterstützen und Missverständnisse zu vermeiden. 😊

**Warum ist das wichtig?**  
Dieser Test stellt sicher, dass der Benutzer bei der Eingabe seiner Daten die richtigen Anweisungen erhält. Wenn Benutzer unklare oder fehlende Anweisungen sehen, könnten sie Fehler machen, was zu Problemen in der Datenverarbeitung führen könnte. 🛠️

Testaufbau und wichtige Hilfsmittel ⛑️
----------------------------------------------------

- **TestWrapper**:  
  Der `TestWrapper` wird verwendet, um React Hook Form in den Tests zu integrieren. Da React Hook Form auf interne Formulareinstellungen zugreift, müssen diese auch während der Tests korrekt bereitgestellt werden. Der `TestWrapper` stellt sicher, dass alle Formulareinstellungen während der Testlaufzeit verfügbar sind.

.. code-block:: react

    const TestWrapper = ({ children }: { children: ReactNode }) => {
      const methods = useForm();
      return <FormProvider {...methods}>{children}</FormProvider>;
    };

.. important::

   Ohne den `TestWrapper` könnten viele Formular-bezogene Tests fehlschlagen, da die Formulare nicht korrekt initialisiert wären.

- **screen.debug()**:  
  Diese Funktion gibt den aktuellen DOM-Baum im Test-Output aus. Es ist ein nützliches Werkzeug, wenn du dich wunderst, warum ein bestimmtes Element im Test nicht gefunden wird. Durch den DOM-Output kannst du schnell erkennen, ob das Element tatsächlich existiert oder warum es nicht gefunden wurde.

.. tip::

   Benutze `screen.debug()`, um einen Blick auf den DOM zu werfen, wenn etwas nicht wie erwartet funktioniert. Das erleichtert die Fehlersuche erheblich. 🔍

Fazit
~~~~~~~~~~~~~~~~~~

Das Testen von Formularen und Komponenten mit **Vitest** und **React Hook Form** ermöglicht es dir, sicherzustellen, dass deine Benutzeroberflächen wie erwartet funktionieren. Durch gezielte Tests auf Überschriften, Eingabefelder und Hilfetexte kannst du sicherstellen, dass das Formular korrekt funktioniert und die Benutzer klare Anweisungen erhalten.

Für weitere Informationen über **Vitest** und dessen Einsatzmöglichkeiten kannst du die offizielle Dokumentation unter https://vitest.dev/ besuchen. 🎉


Arten zum Testen ⚖️
----------------------------

1. Console 🎰
~~~~~~~~~~~~~~~~~

Das Testen über die Konsole ist eine einfache und direkte Methode, um Funktionen und den Programmablauf während der Entwicklung zu überprüfen. Entwicklern steht es frei, `console.log()` oder ähnliche Befehle zu verwenden, um Werte auszugeben und schnelle Rückmeldungen zu erhalten.

**Vorteile**:

- **Schnelligkeit**: Tests über die Konsole sind schnell und erfordern keine komplizierte Einrichtung. Dies ist besonders hilfreich während der Entwicklung, wenn man schnell prüfen möchte, ob bestimmte Werte oder Logiken wie erwartet funktionieren.
- **Einfachheit**: Jeder Entwickler kann sofort mit dem Testen beginnen, da keine zusätzlichen Werkzeuge oder Frameworks erforderlich sind.

**Nachteile**:

- **Manuelle Durchführung**: Konsolentests erfordern manuelle Ausführung und Beobachtung. Es gibt keine Automatisierung, was sie unzuverlässig für langfristige Teststrategien macht.
- **Fehlende Skalierbarkeit**: Bei komplexeren Projekten reicht das Testen über die Konsole nicht aus. Es wird schnell unübersichtlich und ist schwer reproduzierbar.
- **Keine strukturierte Überprüfung**: Es gibt keine systematische Methode, um sicherzustellen, dass alle Funktionen umfassend getestet wurden. Ein Entwickler könnte leicht etwas übersehen.


Der Test wird mit 

.. code-block:: bash

    npm test oder vitest  

.. tip::

   Konsolentests sind besonders nützlich während der Entwicklung und für schnelles Debugging, sollten aber nicht als alleinige Testmethode verwendet werden.

2. UI Testing 🖼️
~~~~~~~~~~~~~~~~~

UI-Testing (User Interface Testing) bezieht sich auf das Testen der grafischen Benutzerschnittstellen, um sicherzustellen, dass die Benutzeroberfläche wie beabsichtigt funktioniert und benutzerfreundlich ist. Dabei wird getestet, ob die Interaktionen mit der Anwendung korrekt verarbeitet werden, indem man z.B. Klicks, Formulareingaben und visuelle Darstellungen überprüft.

**Vorteile**:

- **Benutzernähe**: UI-Tests spiegeln das tatsächliche Nutzerverhalten wider, indem sie Interaktionen mit der Oberfläche testen. Das macht sie besonders nützlich, um sicherzustellen, dass die Anwendung aus Sicht des Endbenutzers funktioniert.
- **Visuelle Validierung**: UI-Tests überprüfen nicht nur die Logik, sondern auch das visuelle Erscheinungsbild und Layout der Anwendung, was sicherstellt, dass Designanforderungen erfüllt werden.
- **Automatisierbarkeit**: Viele UI-Tests können automatisiert werden, z.B. mit Tools wie Selenium, Cypress oder Puppeteer, um menschliche Interaktionen zu simulieren und regelmäßig durchzuführen.

**Nachteile**:

- **Langsam**: UI-Tests sind im Vergleich zu anderen Testmethoden langsamer, da sie oft die Anwendung in einem echten oder simulierten Browser laden und Interaktionen nachahmen müssen.
- **Komplexe Wartung**: Wenn sich die Benutzeroberfläche häufig ändert, müssen UI-Tests regelmäßig aktualisiert werden, was zeitaufwändig sein kann.
- **Fehleranfälligkeit**: UI-Tests können manchmal aus unerwarteten Gründen fehlschlagen, z.B. aufgrund von Zeitverzögerungen, Netzwerkproblemen oder flüchtigen Elementen im DOM. Das macht sie weniger stabil als Unit-Tests.


.. code-block::

    vitest --ui

.. important::

   UI-Tests bieten eine umfassende Möglichkeit, die Benutzererfahrung zu prüfen, sollten jedoch nicht die einzigen Tests in der Teststrategie sein. Kombiniere sie mit Unit- und Integrationstests, um ein vollständiges Bild der Funktionsfähigkeit der Anwendung zu erhalten. 🎯

Unterschiedliche zwischen fireEvent und userEvent 🎆
------------------------------------------------------------

Beim Testen von Benutzerinteraktionen in React-Komponenten stehen zwei Methoden zur Verfügung: **fireEvent** und **userEvent**. Beide dienen dazu, Benutzerinteraktionen wie Klicks, Eingaben oder Tastaturaktionen zu simulieren, jedoch gibt es wesentliche Unterschiede in der Funktionsweise und dem Einsatzgebiet.

fireEvent
~~~~~~~~~~~~~~~~~~~~~~~~~
`fireEvent` ist eine Methode, die direkt DOM-Events auslöst. Diese Methode ist einfacher und schneller, aber sie simuliert die Interaktionen nicht so realistisch wie ein echter Benutzer.

**Syntax**:

.. code-block:: javascript

   fireEvent.click(element);
   fireEvent.change(inputElement, { target: { value: 'Testwert' } });

**Vorteile**:

- **Performance**: Da `fireEvent` die Events direkt im DOM auslöst, ist es schneller als `userEvent`.
- **Einfachheit**: Die API ist einfach und direkt, was für schnelle, einfache Tests geeignet ist.
  
**Nachteile**:

- **Unrealistische Simulation**: Die Simulation von Benutzeraktionen ist weniger realistisch, da keine kompletten Benutzerinteraktionen simuliert werden (z.B. fehlen bei Eingaben Zwischenereignisse wie `keydown` oder `keyup`).
- **Nicht für komplexe Interaktionen geeignet**: Komplexere Benutzeraktionen, wie z.B. Drag-and-Drop oder Tastaturkombinationen, lassen sich mit `fireEvent` nur schwer testen.

### userEvent
~~~~~~~~~~~~~~~~~~~~~~~~~
`userEvent` ist eine Methode aus der Bibliothek **@testing-library/user-event**, die Interaktionen realistischer simuliert, indem sie mehrschrittige Ereignisse erzeugt. So wird z.B. bei der Eingabe von Text nicht nur `change` ausgelöst, sondern auch Ereignisse wie `keydown`, `keypress` und `keyup`.

**Syntax**:

.. code-block:: react

   await userEvent.click(element);
   await userEvent.type(inputElement, 'Testwert');

**Vorteile**:

- **Realistische Simulation**: `userEvent` simuliert Benutzeraktionen so, wie sie tatsächlich im Browser ablaufen, einschließlich aller Zwischenereignisse.
- **Bessere Abdeckung**: Besonders bei komplexeren Interaktionen, wie z.B. Tastatureingaben oder Drag-and-Drop, ist `userEvent` besser geeignet.
- **Unterstützt asynchrone Interaktionen**: Einige Aktionen wie z.B. Texteingaben erfordern `await` und können über einen gewissen Zeitraum simuliert werden, was realistischere Tests ermöglicht.

**Nachteile**:

- **Langsamer**: Da mehr Ereignisse simuliert werden, dauert der Test mit `userEvent` länger als mit `fireEvent`.
- **Komplexität**: Die API ist komplexer und erfordert in manchen Fällen das Warten auf asynchrone Aktionen.

Beispiele für fireEvent und userEvent
---------------------------------------------

Hier ist ein Beispiel für den Einsatz von `fireEvent`, um eine einfache Klick-Interaktion zu testen:

.. code-block:: react

   test('fireEvent example', () => {
     render(<Button>Click me</Button>);
     const button = screen.getByText(/click me/i);
     fireEvent.click(button);
     expect(button).toHaveClass('clicked');
   });

In diesem Beispiel wird `fireEvent.click` verwendet, um einen Klick auf den Button auszulösen und zu prüfen, ob die Klasse des Buttons sich ändert.

Ein ähnliches Beispiel für `userEvent`, das die Texteingabe in einem Formularfeld simuliert:

.. code-block:: react

   test('userEvent example', async () => {
     render(<Input />);
     const input = screen.getByLabelText(/username/i);
     await userEvent.type(input, 'Testbenutzer');
     expect(input).toHaveValue('Testbenutzer');
   });

Hier wird `userEvent.type` verwendet, um die realistische Texteingabe durch den Benutzer zu simulieren, einschließlich der Zwischenereignisse wie `keydown` und `keyup`.

Wann sollte man fireEvent und wann userEvent verwenden?
-------------------------------------------------------

Die Wahl zwischen `fireEvent` und `userEvent` hängt von der Art des Tests ab:

- **fireEvent** eignet sich, wenn du einfache Interaktionen testen möchtest, bei denen es hauptsächlich darum geht, dass ein bestimmtes Ereignis (z.B. ein Klick) ausgelöst wird. Es ist auch ideal, wenn du schnellere Tests benötigst, die keine komplexe Benutzerinteraktionen erfordern.
  
- **userEvent** ist die bessere Wahl für realistische Benutzerszenarien, besonders wenn es um Texteingaben, Drag-and-Drop oder andere mehrstufige Interaktionen geht. Es sollte bevorzugt werden, wenn es wichtig ist, dass alle zugehörigen DOM-Ereignisse simuliert werden.

Fazit
~~~~~~~~~~~~~~~ 

`fireEvent` und `userEvent` sind beide wichtige Werkzeuge für das Testen von Benutzerinteraktionen in Vitest, haben aber unterschiedliche Stärken. `fireEvent` ist schneller und einfacher, während `userEvent` realistischere Simulationen ermöglicht. Durch den gezielten Einsatz beider Methoden lassen sich präzise und aussagekräftige Tests für Benutzeroberflächen erstellen.

Der Tipp mit RenderComponent 💡
---------------------------------------------

Die Funktion `renderComponent` ist eine bewährte Methode, um eine Komponente in einer Testumgebung mit den notwendigen Wrappern bereitzustellen. Dies ermöglicht es dir, die Komponente in isolierten Tests zu rendern und gleichzeitig den Kontext oder Provider (wie Redux oder React Router) zu berücksichtigen.

.. code-block:: react

   const renderComponent = () => {
     render(
       <TestWrapper>
         <CompanyData />
       </TestWrapper>
     );
   };

**Vorteile:**

1. **Wiederverwendbarkeit**: 
   Diese Funktion kann in mehreren Tests verwendet werden, um Konsistenz zu gewährleisten und Redundanz zu vermeiden.

2. **Testkontext**: 
   Der Einsatz eines Wrappers wie `TestWrapper` erlaubt es dir, globale Kontexte (z.B. Thema, Zustand) für deine Komponente bereitzustellen, was für tiefere Integrationstests notwendig ist.

3. **Saubere Struktur**: 
   Durch das Auslagern der Rendering-Logik in eine Hilfsfunktion bleibt der eigentliche Testcode klar und fokussiert auf das Verhalten der Komponente.

**Beispielnutzung in einem Test:**

.. code-block:: react

   test('renders company data correctly', () => {
     renderComponent();
     const companyName = screen.getByText(/Company Name/i);
     expect(companyName).toBeInTheDocument();
   });

Dieser Ansatz stellt sicher, dass die Komponente in einer kontrollierten Umgebung gerendert wird und erleichtert die Testbarkeit komplexer Szenarien.

.. image:: /_static/renderComponent.png