# Quantum Teleportation
```csharp
namespace Quantum.Sample {
    open Microsoft.Quantum.Diagnostics;

    operation Teleport (message : Qubit, target : Qubit) : Unit {
        use register = Qubit();
        // Create some entanglement that we can use to send our message.
        H(register);
        CNOT(register, target);

        // Encode the message into the entangled pair.
        CNOT(message, register);
        H(message);

        // Measure the qubits to extract the classical data we need to
        // decode the message by applying the corrections on
        // the target qubit accordingly.
        if M(message) == One {
            Z(target); 
        }
        // Correction step
        if M(register) == One {
            X(target);
            // Reset register to Zero state before releasing
            X(register);
        }

        Reset(message);
    }

    @EntryPoint()
    operation Main() : Unit {
        use (message, target) = (Qubit(), Qubit());

        // Set the message qubit to a superposition state and inspect the qubits state using the DumpMachine operation.
        H(message);
        DumpMachine();

        // Move the quantum state using the Teleport operation and inspect the qubits state again.
        Teleport(message, target);
        DumpMachine();

        Reset(message);
        Reset(target);
    }
}
```

# Super Dense Coding
```csharp
namespace Quantum.SuperdenseCoding {
    open Microsoft.Quantum.Intrinsic;
    open Microsoft.Quantum.Diagnostics;
    open Microsoft.Quantum.Measurement;

    operation PrepareEntangledPair() : (Qubit, Qubit) {
        use (q1, q2) = (Qubit(), Qubit());
        H(q1);
        CNOT(q1, q2);
        return (q1, q2);
    }

    operation EncodeMessage(message : (Bool, Bool), qubit : Qubit) : Unit {
        let (bit1, bit2) = message;
        if (bit1) {
            Z(qubit);
        }
        if (bit2) {
            X(qubit);
        }
    }

    operation DecodeMessage(q1 : Qubit, q2 : Qubit) : (Bool, Bool) {
        CNOT(q1, q2);
        H(q1);
        let bit1 = MResetZ(q1) == One;
        let bit2 = MResetZ(q2) == One;
        return (bit1, bit2);
    }

    @EntryPoint()
    operation RunSuperdenseCoding() : (Bool, Bool) {
        let message = (true, false);
        Message($"Original message: {message}");

        use q1 = Qubit();
        use q2 = Qubit();
        (q1, q2) = PrepareEntangledPair();
        EncodeMessage(message, q1);

        let decodedMessage = DecodeMessage(q1, q2);
        Message($"Decoded message: {decodedMessage}");

        return decodedMessage;
    }
}
```

# Quantum Cryptography
```csharp
mespace Quantum.Sample {
   open Microsoft.Quantum.Intrinsic;
   open Microsoft.Quantum.Measurement;
   open Microsoft.Quantum.Arrays;

   operation PrepareQubits(bases: Bool[], bits: Bool[]) : Qubit[] {
       let n = Length(bases);
       use qubits = Qubit[n];
       for i in 0..n-1 {
           if (bases[i]) {
               H(qubits[i]);
           }
           if (bits[i]) {
               X(qubits[i]);
           }
       }
       return qubits;
   }

   operation MeasureQubits(bases: Bool[], qubits: Qubit[]) : Bool[] {
       let n = Length(bases);
       mutable results = new Bool[n];
       for i in 0..n-1 {
           if (bases[i]) {
               H(qubits[i]);
           }
           set results w/= i <- MResetZ(qubits[i]) == One;
       }
       return results;
   }

   @EntryPoint()
   operation BB84Protocol() : (Bool[], Bool[]) {
       let n = 10;

       // Alice generates random bits and bases
       let aliceBits = [true, false, true, false, false, true, true, false, true, false];
       let aliceBases = [false, true, false, true, true, false, true, false, true, true];

       // Alice prepares qubits based on her bits and bases
       let aliceQubits = PrepareQubits(aliceBases, aliceBits);

       // Bob generates random bases
       let bobBases = [true, false, true, false, true, true, false, true, false, false];

       // Bob measures Alice's qubits using his bases
       let bobBits = MeasureQubits(bobBases, aliceQubits);

       // Alice and Bob compare their bases and keep the bits where their bases match
       mutable aliceKey = [];
       mutable bobKey = [];
       for i in 0..n-1 {
           if (aliceBases[i] == bobBases[i]) {
               set aliceKey += [aliceBits[i]];
               set bobKey += [bobBits[i]];
           }
       }

       // Output Alice's and Bob's keys
       return (aliceKey, bobKey);
   }

}
```
