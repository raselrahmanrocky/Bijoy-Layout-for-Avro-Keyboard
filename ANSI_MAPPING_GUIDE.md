# Custom ANSI Mapping Guide

This guide explains how to customize and create ANSI font mapping files (`.json`) for this Bengali Keyboard Engine. It allows you to map Unicode Bengali characters to legacy ANSI font glyphs (such as SutonnyMJ) and resolve rendering issues with conjuncts (যুক্তবর্ণ) and vowel signs (কার-চিহ্ন).

---

## JSON Structure

A valid mapping JSON file must contain four mandatory sections:
1. `Constants` (Object)
2. `PreReplacements` (Array)
3. `FullFormReplacements` (Array)
4. `PostReplacements` (Array)

---

## 1. Constants (Visual Glyph Map)

Legacy ANSI fonts use different English characters (glyphs) to represent the same vowel sign based on the preceding consonant's shape. The engine dynamically swaps these variants to ensure perfect rendering.

### Vowel Sign "U-Kar" (ু) Variants:
*   `A_UKar1` (Default: `"z"`): Standard bottom-aligned U-Kar. Used under flat consonants (e.g., কু, চু, তু).
*   `A_UKar2` (Default: `"y"`): Hanging U-Kar. Used under consonants with bottom loops or complex shapes (e.g., গু, শু, মু, হু, ন্দু).
*   `A_UKar3` (Default: `"#$00AD"`): Special loop U-Kar used specifically for **'ড়ু'** and **'ঢ়ু'**.
*   `A_UKar4` (Default: `"#$00E6"`): Special wrapped U-Kar used for the default style of **"রু"** (looping from the middle of the 'Ra').

### Vowel Sign "UU-Kar" (ূ) Variants:
*   `A_UUKar1` (Default: `"#$201A"`): Standard bottom-aligned UU-Kar (e.g., কূ, চূ, তূ).
*   `A_UUKar2` (Default: `"~"`): Hanging UU-Kar (e.g., গূ, শূ, মূ).
*   `A_UUKar3` (Default: `"#$0192"`): Special wrapped UU-Kar used for **"রূ"** (looping from the middle of the 'Ra').

---

## 2. Replacements Sections

These arrays handle the substitution of conjunct letters (যুক্তবর্ণ) and custom overrides.

*   **`PreReplacements`:** Applied at the very beginning of the conversion process.
*   **`FullFormReplacements`:** Applied right after vowel rearrangements. Use this to define how standard conjuncts map to single ANSI glyphs.
*   **`PostReplacements`:** Applied at the very end of the conversion to fix minor visual glitches (e.g., fixing specific character overlaps).

### Rule Schema:
Every item in these arrays must strictly contain a non-empty `"Key"` and `"Value"`.
```json
{
  "Key": "ক্ক",
  "Value": "°"
}
```


### Bangla Instruction:
বিজয় তথা সুতন্বীএমজে (SutonnyMJ) ফন্টে একই উ-কার (ু) এবং ঊ-কার (ূ) বিভিন্ন ব্যঞ্জনবর্ণের নিচে বিভিন্ন আকৃতিতে প্রদর্শিত হয় [1, 2]। ফন্ট ডিজাইনের সুবিধার জন্য এই বিভিন্ন আকৃতি বা রূপগুলোকে আলাদা আলাদা ইংরেজি অক্ষরের অবস্থানে রাখা হয়েছে [1]。 

আপনার কোডে থাকা কন্সট্যান্টগুলোর বিস্তারিত অর্থ নিচে সহজ ভাষায় বুঝিয়ে দেওয়া হলো [2, 3]:

---

### **উ-কার (ু) এর বিভিন্ন রূপ:**

*   **`A_UKar1: Char = #$7A;` (যার ইংরেজি অক্ষর হলো `z`)**
    *   **অর্থ:** এটি হলো **সাধারণ বা সোজা উ-কার** [2, 3]। কীবোর্ড থেকে সরাসরি `z` টাইপ করলে এটি আসে। এটি সাধারণ অক্ষরের নিচে বসে (যেমন: ক্কু, কু, চু, তু) [2]。
*   **`A_UKar2: Char = #$79;` (যার ইংরেজি অক্ষর হলো `y`)**
    *   **অর্থ:** এটি হলো **ঝুলে থাকা উ-কার (Hanging U-Kar)** [2, 3]। এটি সাধারণত যে অক্ষরের নিচে অতিরিক্ত অংশ বা গোল বৃত্ত থাকে, সেগুলোর নিচে ঝুলে বসে যাতে দেখতে সুন্দর লাগে (যেমন: গু, শু, মু, হু, ন্দু) [1, 2]।
*   **`A_UKar3: Char = #$2013;` (যার ইংরেজি অক্ষর হলো En Dash `–`)**
    *   **অর্থ:** এটি শুধুমাত্র **'ড়ু'** এবং **'ঢ়ু'** লেখার জন্য ব্যবহৃত বিশেষ উ-কার [2, 3]। এটি 'ড়' বা 'ঢ়'-এর নিচের বিন্দুর সাথে গোল হয়ে যুক্ত হয় [2]。
*   **`A_UKar4: Char = #$201C;` (যার ইংরেজি ক্যারেক্টার হলো ডাবল কোটেশন `“`)**
    *   **অর্থ:** এটি শুধুমাত্র **"রু"** লেখার জন্য ব্যবহৃত বিশেষ উ-কার [2, 3]। র-এর আন্সি কোড `i` এর সাথে এটি বসে র-এর মাঝখান থেকে গোল হয়ে আসা সুন্দর রূপটি তৈরি করে [1, 2]।

---

### **ঊ-কার (ূ) এর বিভিন্ন রূপ:**

*   **`A_UUKar1: Char = #$201A;` (যার ইংরেজি ক্যারেক্টার হলো `‚`)**
    *   **অর্থ:** এটি হলো **সাধারণ বা সোজা ঊ-কার** [2, 3]। এটি সাধারণ অক্ষরের নিচে বসে (যেমন: কূ, চূ, তূ) [2]।
*   **`A_UUKar2: Char = #$7E;` (যার ইংরেজি ক্যারেক্টার হলো টিল্ডা `~`)**
    *   **অর্থ:** এটি হলো **ঝুলে থাকা ঊ-কার** [2, 3]। এটি উ-কারের মতোই নির্দিষ্ট কিছু অক্ষরের নিচে ঝুলে বসে (যেমন: গূ, শূ, মূ) [1, 2]।
*   **`A_UUKar3: Char = #$192;` (যার ইংরেজি ক্যারেক্টার হলো ফ্লোরিন `ƒ`)**
    *   **অর্থ:** এটি শুধুমাত্র **"রূ"** লেখার জন্য ব্যবহৃত বিশেষ ঊ-কার [2, 3]। র-এর আন্সি কোড `i` এর সাথে এটি বসে র-এর মাঝখান থেকে গোল হয়ে আসা সুন্দর ঊ-কার তৈরি করে [1, 2]।

---

**সংক্ষেপে:**
যেকোনো ইউনিকোড থেকে আন্সি (বিজয়) কনভার্টার যখন কোনো উ-কার বা ঊ-কার প্রসেস করে, তখন ফন্ট রেন্ডারিং নিখুঁত করার জন্য আগের অক্ষরের ধরন দেখেই এই কন্সট্যান্টগুলো থেকে সঠিক গ্লিফটি সিলেক্ট করে আউটপুটে পাঠায় [1, 2]।


