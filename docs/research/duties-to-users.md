# Duties to learners: privacy, consent, deletion, age, and copyright

**These findings are not legal advice.** They summarize what laws, regulators, and AI providers say in their own published texts. Before you rely on a finding, ask a lawyer who practices in the country concerned.

- Research date: 2026-11-26.
- Question: [issue 7](https://github.com/calmxz/Suro/issues/7). Which duties does Suro have to its learners as an adults-only app that stores materials?
- Scope: the Philippines first, then learners in the EU, the UK, and the US.
- Method: each claim cites the section or article that owns it, with a link. "Not verified" marks a claim that I could not check against the owning text.
- In this file, "you" means the person who runs Suro.

## Short answer

- The Philippine Data Privacy Act applies to you at any size. The exclusion for personal and household affairs does not cover a service for other people.
- You must give a privacy notice, use a lawful basis, and keep data only as long as needed.
- You must secure the data and honor requests for access, correction, and erasure.
- You must report a qualifying breach to the National Privacy Commission (NPC) and to learners within 72 hours. Login data counts as data that can enable identity fraud, which is one of three conditions.
- NPC registration has three triggers that do not depend on size. They are profiling, likely risk to rights, and processing that is "not occasional" in older rules. Whether Suro meets them is open.
- If you do not register, an NPC circular requires a notarized sworn declaration instead.
- No law I found prescribes how an adults-only gate must check age. An NPC advisory warns that self-declaration alone may be inadequate for high-risk processing.
- The gate matters because Philippine, US, UK, and AI-provider rules attach extra duties to services that children are likely to use.
- A learner's material can be under copyright. Philippine fair use and private-study copying are narrow. A notice-and-removal process reduces your exposure in the Philippines and the US.
- Sending a material to an AI provider is a disclosure to a processor. The Anthropic API terms give no training, deletion within 30 days, and a data processing addendum. Whether that addendum meets every Philippine requirement is not verified.
- A Claude Max subscription falls under the consumer terms instead. Those terms forbid sharing the account and limit automated access. I found no data processing addendum in them.

## 1. Philippines: the Data Privacy Act

### Does the act apply to Suro

- The act covers "any natural and juridical person involved in personal information processing" ([RA 10173, Sec. 4](https://privacy.gov.ph/data-privacy-act/)).
- It also reaches processing outside the Philippines that relates to a Philippine citizen or resident ([RA 10173, Sec. 6(a)](https://privacy.gov.ph/data-privacy-act/)).
- A personal information controller (PIC) controls the collection and use of personal information ([RA 10173, Sec. 3(h)](https://privacy.gov.ph/data-privacy-act/)).
- The PIC definition excludes an individual who processes data for "personal, family or household affairs" ([RA 10173, Sec. 3(h)(2)](https://privacy.gov.ph/data-privacy-act/)). Suro serves strangers, so this exclusion does not appear to apply. This is my reading, not an NPC ruling.
- Personal information is any information from which a person's identity "is apparent or can be reasonably and directly ascertained" ([RA 10173, Sec. 3(g)](https://privacy.gov.ph/data-privacy-act/)). A learner account, a stored material, and mastery records tied to an account all fit.
- A material can also hold personal information about other people, such as names in class notes.

### Sensitive personal information: an open question for Suro

The act treats some information as sensitive. The list includes information about a person's "age" and "education" ([RA 10173, Sec. 3(l)(1) and (2)](https://privacy.gov.ph/data-privacy-act/)).

This matters for two Suro features:

- **The age gate.** If you store a birth date, you store information about age. A yes-or-no answer to "Are you 18 or older?" may or may not count. I found no NPC text that settles this.
- **Mastery records.** A record of what a learner understands may count as information about education. I found no NPC text that settles this either.

If either counts as sensitive, these stricter rules apply:

- Processing is prohibited unless an exception applies. The main exception is consent "specific to the purpose prior to the processing" ([RA 10173, Sec. 13(a)](https://privacy.gov.ph/data-privacy-act/)).
- Sensitive information processed through a contract is treated as consent-based processing under Sec. 13(a) ([NPC Circular 2023-04, Sec. 19](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- A breach of sensitive information meets the first condition for mandatory notification ([RA 10173, Sec. 20(f)](https://privacy.gov.ph/data-privacy-act/)).
- Penalties are higher, for example 3 to 6 years for unauthorized processing ([RA 10173, Sec. 25(b)](https://privacy.gov.ph/data-privacy-act/)).

### Lawful basis and consent

- You may process personal information only on one of six grounds. They include consent, a contract with the person, and legitimate interests ([RA 10173, Sec. 12](https://privacy.gov.ph/data-privacy-act/)).
- Consent must be "freely given, specific, informed" and "evidenced by written, electronic or recorded means" ([RA 10173, Sec. 3(b)](https://privacy.gov.ph/data-privacy-act/)).
- If another lawful basis applies, you do not need consent for that processing ([NPC Circular 2023-04, Sec. 5(C)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- To avoid consent fatigue, you must identify the lawful basis before you collect data. If another basis applies, a consent request is unnecessary ([Circular 2023-04, Sec. 9(B)(1)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).

If you rely on consent, Circular 2023-04 sets these rules:

- Consent must be granular. For unrelated purposes, the learner selects each purpose ([Sec. 8](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- Consent for processing that the service does not need must not be bundled with the service ([Sec. 8(B)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- Vague or blanket consent is invalid ([Sec. 8(C)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- Consent needs a clear assenting action. Silence and implied consent are invalid ([Sec. 10 and 10(A)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- You must not use deceptive design to get consent ([Sec. 7(A)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- You must be able to show that the learner consented, with enough evidence ([Sec. 11 and 12(A)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- If the purpose or scope of processing changes considerably, the old consent is no longer valid ([Sec. 21(A)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- A learner's waiver of data privacy rights is void ([Sec. 20](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).

### Notice: what you must tell learners

- You must give a privacy notice for any processing, whatever the lawful basis ([Circular 2023-04, Sec. 3(D)(5)(a)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- Before data enters your system, you must state these items ([RA 10173, Sec. 16(b)](https://privacy.gov.ph/data-privacy-act/)):
  - what data you collect,
  - the purposes,
  - the scope and method of processing,
  - the recipients, such as an AI provider,
  - your identity and contact details,
  - how long you keep the data, and
  - the learner's rights, including the right to complain to the NPC.
- The learner has a right to know about automated decision-making and profiling ([IRR, Sec. 34(a)(1)](https://privacy.gov.ph/implementing-rules-regulations-data-privacy-act-2012/)).
- If you profile, you must inform learners of its existence and details before their data enters your system ([Circular 2023-04, Sec. 18](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- For AI systems, you must be able to explain the purpose, the inputs, the risks, and the expected output ([NPC Advisory 2024-04, Sec. 2(A)](https://privacy.gov.ph/wp-content/uploads/2025/02/Advisory-2024.12.19-Guidelines-on-Artificial-Intelligence-w-SGD.pdf)).
- Use clear, plain language. Use a short layered notice that links to the full details ([Circular 2023-04, Sec. 3(B) and 3(C)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).

### Rights and deletion

The learner has these rights:

- Access to their data, its recipients, and how it was processed ([RA 10173, Sec. 16(c)](https://privacy.gov.ph/data-privacy-act/)).
- Correction of inaccurate data ([Sec. 16(d)](https://privacy.gov.ph/data-privacy-act/)).
- Erasure or blocking ([Sec. 16(e)](https://privacy.gov.ph/data-privacy-act/)). The IRR lists the grounds. They include data no longer needed, withdrawn consent, and unlawful processing ([IRR, Sec. 34(e)](https://privacy.gov.ph/implementing-rules-regulations-data-privacy-act-2012/)).
- Objection, including to profiling ([IRR, Sec. 34(b)](https://privacy.gov.ph/implementing-rules-regulations-data-privacy-act-2012/)).
- A copy in a structured, commonly used electronic format ([RA 10173, Sec. 18](https://privacy.gov.ph/data-privacy-act/)).
- Damages for misuse of their data ([Sec. 16(f)](https://privacy.gov.ph/data-privacy-act/)).

On deletion and retention:

- Keep data only as long as the purpose needs it ([RA 10173, Sec. 11(e)](https://privacy.gov.ph/data-privacy-act/)).
- Data "shall not be retained in perpetuity in contemplation of a possible future use yet to be determined" ([IRR, Sec. 19(d)](https://privacy.gov.ph/implementing-rules-regulations-data-privacy-act-2012/)).
- If a learner withdraws consent and no other basis applies, you must stop processing and delete the data ([Circular 2023-04, Sec. 13](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- Withdrawal must be at least as easy as giving consent, through the same interface ([Sec. 13(A) and 13(B)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).
- For AI systems, you must give effect to erasure and objection rights. If a request is not feasible, tell the learner why ([Advisory 2024-04, Sec. 3(A)](https://privacy.gov.ph/wp-content/uploads/2025/02/Advisory-2024.12.19-Guidelines-on-Artificial-Intelligence-w-SGD.pdf)).
- The fact that data is in a data set does not make a rights request unreasonable ([Advisory 2024-04, Sec. 3(C)](https://privacy.gov.ph/wp-content/uploads/2025/02/Advisory-2024.12.19-Guidelines-on-Artificial-Intelligence-w-SGD.pdf)).

A deletion in Suro does not delete copies that an AI provider still holds. See section 4 for provider retention periods.

### Security and breaches

- You must use "reasonable and appropriate" organizational, physical, and technical measures ([RA 10173, Sec. 20(a)](https://privacy.gov.ph/data-privacy-act/)).
- The right level of security depends on "the size of the organization" and "the cost of security implementation", among other factors ([Sec. 20(c)](https://privacy.gov.ph/data-privacy-act/)). This is the main place where size lowers a duty.
- You must make sure third parties that process data for you also secure it ([Sec. 20(d)](https://privacy.gov.ph/data-privacy-act/)).

[NPC Circular 16-03](https://privacy.gov.ph/wp-content/uploads/2022/01/sgd-npc-circular-16-03-personal-data-breach-management.pdf) sets the breach rules:

- Have a security incident policy (Sec. 4) and a breach response team (Sec. 5). The team needs at least one member who can make immediate decisions.
- Keep a written record of every incident (Sec. 9). Send the NPC an annual summary of all incidents (Sec. 22).
- Notification is required when all three conditions hold (Sec. 11):
  - the data is sensitive or can enable identity fraud,
  - an unauthorized person may have acquired it, and
  - the acquisition is likely to cause a real risk of serious harm.
- "Usernames, passwords and other login data" count as information that can enable identity fraud (Sec. 11(A)). This meets the first condition only. The other two must also hold.
- You notify even if a processor, such as an AI provider or a host, had the breach (Sec. 15).
- If you are unsure whether to notify, you must consider whether the breach involves at least 100 individuals (Sec. 13(B)).
- Notify the NPC and affected learners within 72 hours of knowledge or reasonable belief ([IRR, Sec. 38(a)](https://privacy.gov.ph/implementing-rules-regulations-data-privacy-act-2012/); Circular 16-03 summary table).
- Failure to notify is presumed if the NPC does not hear from you within five days (Sec. 20).
- The full breach report is due within five days of discovery, unless the NPC acts on a request ([NPC Advisory 2026-02, Sec. 2(C)](https://privacy.gov.ph/wp-content/uploads/2026/05/Advisory-Clarification-on-the-Submission-of-Personal-Data-Breach-Notification-through-DBNMS_11-May-2026.pdf)). NPC inaction is not approval (Sec. 2(D)).

### Penalties apply to individuals

The act's penalties are criminal and apply to "persons". They are not limited to companies.

- Unauthorized processing: 1 to 3 years and PHP 500,000 to 2,000,000 ([RA 10173, Sec. 25(a)](https://privacy.gov.ph/data-privacy-act/)).
- Negligent access: the same range ([Sec. 26(a)](https://privacy.gov.ph/data-privacy-act/)).
- Concealing a breach: 1 year and 6 months to 5 years ([Sec. 30](https://privacy.gov.ph/data-privacy-act/)).
- The maximum penalty applies when at least 100 persons are affected ([Sec. 35](https://privacy.gov.ph/data-privacy-act/)).

### Registration, data protection officer, and scale

- Every PIC must designate an individual accountable for compliance ([RA 10173, Sec. 21(b)](https://privacy.gov.ph/data-privacy-act/); [IRR, Sec. 26(a)](https://privacy.gov.ph/implementing-rules-regulations-data-privacy-act-2012/)). This has no size threshold.

[NPC Circular 2022-04](https://privacy.gov.ph/wp-content/uploads/2023/05/Circular-2022-04-1.pdf) (dated 5 December 2022) governs registration:

- Registration is mandatory if any of these holds (Sec. 5):
  - you employ 250 or more persons,
  - you process sensitive personal information of 1,000 or more individuals, or
  - your processing will likely pose a risk to rights and freedoms.
- A system that involves "automated decision-making or profiling shall, in all instances, be registered" (Sec. 5(A)).
- "Profiling" includes automated processing used "to evaluate, analyze, or predict" a person's "performance" (Sec. 2(L)).
- If you use a system as a service, you register it and state that a service provider does the processing (Sec. 5(B)).
- If registration is not mandatory and you do not register voluntarily, you "shall submit a sworn declaration" (Sec. 6, Annex 1).
- Register a new system within 20 days of starting it (Sec. 7).
- The supporting papers listed in Sec. 11 cover government agencies, corporations, one person corporations, partnerships, sole proprietorships, and foreign entities.
- An individual professional registers as their own data protection officer (Sec. 5(C) and Sec. 8). The circular defines an individual professional as a self-employed person who earns income from a profession (Sec. 2(I)).
- I found no registration route for a private person with no business registration.
- The NPC FAQ confirms the sworn declaration rule and the Sec. 5 criteria ([NPC FAQ](https://privacy.gov.ph/pips-and-pics/faqs/)).
- PICs that do any automated processing must notify the NPC under the registration circular ([Circular 2023-04, Sec. 18(A)](https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf)).

The older IRR exemption for organizations under 250 persons has a further condition. It fails if the processing "is not occasional" ([IRR, Sec. 47](https://privacy.gov.ph/implementing-rules-regulations-data-privacy-act-2012/)). A continuous web service is not occasional.

**Open questions about registration.** Three triggers do not depend on size:

1. **Profiling.** Suro evaluates what each learner understands and stores it as mastery. That looks like evaluating "performance" under Sec. 2(L) of Circular 2022-04. If it is profiling, registration is mandatory.
2. **Likely risk.** Sec. 5 also requires registration when processing "will likely pose a risk" to rights and freedoms. The circular does not define the threshold in the parts I read.
3. **Not occasional.** IRR Sec. 47 uses this test, but Circular 2022-04 Sec. 5 does not. Which text governs a conflict is Not verified.

If none of the three applies, you file the sworn declaration instead. I found no NPC ruling on an adaptive tutor.

### AI systems: the NPC's own guidance

[NPC Advisory 2024-04](https://privacy.gov.ph/wp-content/uploads/2025/02/Advisory-2024.12.19-Guidelines-on-Artificial-Intelligence-w-SGD.pdf) (19 December 2024) applies the act to AI systems:

- You stay accountable when you outsource processing to a processor (Sec. 2(B)).
- The NPC lists privacy impact assessments and privacy by design among expected governance measures (Sec. 2(B)(2)).
- If automated decisions pose a significant risk, provide human intervention and a way to contest them (Sec. 2(B)(2)(a)).
- Exclude by default any personal data that does not improve the system (Sec. 2(E)).
- Choose the most appropriate lawful basis before processing (Sec. 2(F)).
- Do not overstate the role of AI to learners (Sec. 2(C)).

## 2. The adults-only gate

### What the law says about age

- In the Philippines, majority starts at 18 ([Family Code, Art. 234, as amended by RA 6809](https://lawphil.net/statutes/repacts/ra1989/ra_6809_1989.html), unofficial copy).
- I found no Philippine law that prescribes an age-check method for a general learning app. This is not an exhaustive search. Mark it "Not verified".
[NPC Advisory 2024-03](https://privacy.gov.ph/wp-content/uploads/2024/12/Advisory-2024.12.17-Guidelines-on-Child-Oriented-Transparency-w-SGD.pdf) (17 December 2024) covers children's data:

- It treats a person below 18 as a child (recitals).
- It applies to products "specifically intended for children or likely to be accessed by children" (Sec. 1).
- Before launch, such a product needs a child privacy impact assessment (Sec. 2(A)).
- You "may" use age assurance. Relying "solely on users' self-declaration may be inadequate for high-risk processing activities" (Sec. 2(B)(1)).
- Data you collect for age assurance must follow the general privacy principles and have a lawful basis (Sec. 2(B)(1)).
- You choose the age-assurance method, "if any", by a risk-based approach (Sec. 5).
- If a service contemplates children as data subjects, even when it is also for adults, you must give child-friendly notices (Sec. 3(B)).

So the gate matters in the Philippines too. It is the main evidence that Suro is not "likely to be accessed by children".

### Why the gate matters in other countries

**United States (COPPA):**

- A "child" is a person under 13 ([16 CFR 312.2](https://www.ecfr.gov/current/title-16/chapter-I/subchapter-C/part-312)).
- The rule binds a service "directed to children". It also binds any operator with "actual knowledge" that it collects a child's personal information ([16 CFR 312.3](https://www.ecfr.gov/current/title-16/chapter-I/subchapter-C/part-312)).
- The FTC weighs subject matter, visual content, animated characters, and audience evidence to decide if a service is directed to children ([16 CFR 312.2](https://www.ecfr.gov/current/title-16/chapter-I/subchapter-C/part-312)).
- The rule "does not require operators to ask the age of visitors" ([FTC COPPA FAQ](https://www.ftc.gov/business-guidance/resources/complying-coppa-frequently-asked-questions)).
- An operator that screens age "in a neutral fashion" may rely on the age entered, even if it is false ([FTC COPPA FAQ](https://www.ftc.gov/business-guidance/resources/complying-coppa-frequently-asked-questions)).
- If you later learn that a learner is under 13, the notice and parental consent duties apply ([FTC COPPA FAQ](https://www.ftc.gov/business-guidance/resources/complying-coppa-frequently-asked-questions)).
- A terms-of-service ban on children does not stop a finding that a service is directed to children ([FTC COPPA FAQ](https://www.ftc.gov/business-guidance/resources/complying-coppa-frequently-asked-questions)).
- Foreign services must comply if they are directed to US children or knowingly collect from them ([FTC COPPA FAQ](https://www.ftc.gov/business-guidance/resources/complying-coppa-frequently-asked-questions)).
- An "operator" runs a service "operated for commercial purposes" ([16 CFR 312.2](https://www.ecfr.gov/current/title-16/chapter-I/subchapter-C/part-312)). Whether a free portfolio project counts is Not verified.

**United Kingdom (Children's code):**

- The ICO code covers online services "likely to be accessed by children" in the UK. It is not limited to services designed for children ([ICO, services covered](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/childrens-information/childrens-code-guidance-and-resources/age-appropriate-design-a-code-of-practice-for-online-services/services-covered-by-this-code/)).
- "Likely" means "more probable than not" (same page).
- For an adult-only service, the ICO says to focus on how you prevent access. In that case the code does not apply (same page).
- If you decide the code does not apply, record and support your reasons (same page).
- A tutor is the kind of service that children may want to use. This makes the gate and its record more important.

**European Union:**

- If you rely on consent for an online service offered directly to a child, the default age is 16. Below that, a parent must consent ([GDPR, Art. 8(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)).

### AI providers also require an adult audience

- Google: you must be 18 or older to use the Gemini API. You must not use it in an app "likely to be accessed by individuals under the age of 18". See the [Gemini API Additional Terms](https://ai.google.dev/gemini-api/terms), effective 23 March 2026.
- Anthropic consumer plans: the minimum age is 18 ([Consumer Terms, Sec. 2](https://www.anthropic.com/legal/consumer-terms)).
- Anthropic API: products that serve minors must follow extra guidelines ([Usage Policy](https://www.anthropic.com/legal/aup)).
- OpenAI: you must not let minors use the services without parental consent ([Services Agreement, Sec. 3.3(c)](https://openai.com/policies/services-agreement/)).

### What the findings imply a gate must do

No text above prescribes a gate design. Taken together, they point to these properties:

1. State in your terms that Suro is for persons 18 and older.
2. Ask for age neutrally, so the question does not hint at the answer that passes (FTC COPPA FAQ).
3. Block sign-up below 18, because the Google terms and the ICO guidance focus on preventing access.
4. Keep only what the gate needs. A stored birth date is information about age, which the Philippine act lists as sensitive ([Sec. 3(l)(1)](https://privacy.gov.ph/data-privacy-act/); [Sec. 11(d)](https://privacy.gov.ph/data-privacy-act/) requires data to be "not excessive").
5. Decide what happens when you learn that a learner is under 18. No text I read sets this rule for an adult-only service. For a learner under 13, COPPA notice and parental consent duties apply (FTC COPPA FAQ).
6. Weigh whether self-declaration is enough for Suro's risk level (NPC Advisory 2024-03, Sec. 2(B)(1)).
7. Keep a record of how the gate works and why, as the ICO asks.
8. Avoid child-oriented design, which the FTC uses as evidence of a child audience ([16 CFR 312.2](https://www.ecfr.gov/current/title-16/chapter-I/subchapter-C/part-312)).

## 3. Copyright in a material

### Philippines: the Intellectual Property Code

Citations use an unofficial copy of [RA 8293](https://lawphil.net/statutes/repacts/ra1997/ra_8293_1997.html) and its [2013 amendment, RA 10372](https://lawphil.net/statutes/repacts/ra2013/ra_10372_2013.html). The Official Gazette blocked automated access.

- A work is protected "from the moment of their creation" ([Sec. 172.1](https://lawphil.net/statutes/repacts/ra1997/ra_8293_1997.html)). Registration is not needed. Most books, notes, and slides that learners upload are protected.
- The owner has the exclusive right to reproduce the work ([Sec. 177.1](https://lawphil.net/statutes/repacts/ra1997/ra_8293_1997.html)). The owner also controls "abridgment" and other transformation ([Sec. 177.2](https://lawphil.net/statutes/repacts/ra1997/ra_8293_1997.html)).
- Fair use for "teaching", "scholarship", and "research" is not infringement ([Sec. 185.1](https://lawphil.net/statutes/repacts/ra1997/ra_8293_1997.html)).
- Courts weigh four factors ([Sec. 185.1](https://lawphil.net/statutes/repacts/ra1997/ra_8293_1997.html)):
  - the purpose, including commercial versus "non-profit educational" use,
  - the nature of the work,
  - the amount used, and
  - the effect on the market for the work.
- A natural person may make one private copy of a published work "exclusively for research and private study" ([Sec. 187.1](https://lawphil.net/statutes/repacts/ra1997/ra_8293_1997.html)).
- That permission does not cover an entire book or a substantial part of one by reprographic means. It also excludes a compilation of data and most computer programs ([Sec. 187.2](https://lawphil.net/statutes/repacts/ra1997/ra_8293_1997.html)).
- Sec. 187.1 names a "natural person" making the copy. Whether it covers a copy that a service makes for the learner is Not verified.

Liability for what learners do:

- You infringe if you benefit from another person's infringement after "notice of the infringing activity". This applies when you have "the right and ability to control" that person ([Sec. 216(b), as amended by RA 10372, Sec. 22](https://lawphil.net/statutes/repacts/ra2013/ra_10372_2013.html)).
- You also infringe if you knowingly induce or materially contribute to infringement ([Sec. 216(c)](https://lawphil.net/statutes/repacts/ra2013/ra_10372_2013.html)).
- Notice is the trigger in Sec. 216(b). A way to receive notices and remove a material is the practical response.

A limited shield exists for service providers:

- A service provider that "merely provides access" is not liable for infringement in material it stores ([Electronic Commerce Act, RA 8792, Sec. 30](https://lawphil.net/statutes/repacts/ra2000/ra_8792_2000.html), unofficial copy).
- The conditions are: no actual knowledge, no financial benefit directly from the infringement, and no direct infringement by the provider (same section).
- "Service provider" includes a provider of the technical means to store electronic files and make them accessible ([RA 8792, Sec. 5(j)](https://lawphil.net/statutes/repacts/ra2000/ra_8792_2000.html)).
- Suro does more than store a material. It sends the material to an AI model and produces explanations from it. Whether Sec. 30 still applies is Not verified.

Reform status: news reports describe pending bills to amend the IP Code. I did not check whether any amendment after RA 10372 has passed. Not verified.

### United States: DMCA safe harbor

- A service provider that meets set conditions is not liable for money damages for stored infringing material. The statute covers "storage at the direction of a user" ([17 U.S.C. 512(c)(1)](https://www.law.cornell.edu/uscode/text/17/512), unofficial copy).
- The conditions: no actual knowledge, no financial benefit directly from infringement it can control, and fast removal after a valid notice (same subsection).
- You designate an agent with the US Copyright Office to receive takedown notices ([DMCA Designated Agent Directory](https://www.copyright.gov/dmca-directory/)).
- The fee for a "Designation of agent under 17 U.S.C. 512(c)(2)" is USD 6 ([37 CFR 201.3](https://www.ecfr.gov/current/title-37/chapter-II/subchapter-A/part-201/section-201.3)).
- A designation expires after three years unless you renew it ([37 CFR 201.38(c)(4)](https://www.ecfr.gov/current/title-37/chapter-II/subchapter-A/part-201/section-201.38)).
- You must adopt and reasonably implement a policy to end the accounts of repeat infringers ([17 U.S.C. 512(i)(1)(A)](https://www.law.cornell.edu/uscode/text/17/512)).
- The safe harbor has no size threshold.
- Whether AI processing of a stored material stays inside "storage at the direction of a user" is Not verified.

### Not researched

- Copyright in Suro's generated explanations.
- EU and UK hosting liability rules.

## 4. Sending a material to an AI provider

Sending a material to a model provider has two sides. On the privacy side, it is a disclosure to a processor. On the copyright side, it is a reproduction of the material.

### Philippine rules on processors

- You may subcontract processing, but you stay responsible for safeguards ([RA 10173, Sec. 14](https://privacy.gov.ph/data-privacy-act/)).
- You stay accountable for data sent to a third party, "whether domestically or internationally" ([Sec. 21](https://privacy.gov.ph/data-privacy-act/)).
- You must use "contractual or other reasonable means" to get comparable protection ([Sec. 21(a)](https://privacy.gov.ph/data-privacy-act/)).
- A processor contract must state the subject, duration, purpose, data types, and the location of processing ([IRR, Sec. 44(a)](https://privacy.gov.ph/implementing-rules-regulations-data-privacy-act-2012/)).
- The NPC publishes model contractual clauses for cross-border transfers (NPC Advisory 2024-01, listed on the [NPC advisories page](https://privacy.gov.ph/pips-and-pics/advisories-circulars/)). I did not read it. Not verified.

### Anthropic API (Commercial Terms)

- The terms let you use the API "to power products and services" for your own customers ([Commercial Terms, Sec. A.1](https://www.anthropic.com/legal/commercial-terms)).
- "Anthropic may not train models on Customer Content from Services" ([Sec. B](https://www.anthropic.com/legal/commercial-terms)).
- The Data Processing Addendum is part of the terms ([Sec. C](https://www.anthropic.com/legal/commercial-terms)).
- You warrant that you have "all rights and permissions required to submit Inputs" ([Sec. L.1](https://www.anthropic.com/legal/commercial-terms)). This puts the copyright burden for a material on you.
- You must defend Anthropic against third-party claims about your inputs ([Sec. K.2](https://www.anthropic.com/legal/commercial-terms)).
- The Philippines is a supported region ([Supported Regions Policy](https://www.anthropic.com/supported-countries)).

The [Data Processing Addendum](https://www.anthropic.com/legal/data-processing-addendum) says:

- You are the controller and Anthropic is the processor (Sec. B.1).
- Anthropic processes only on your written instructions (Sec. B.2).
- Anthropic gives notice of a new subprocessor, and you have 15 days to object (Sec. C.3).
- Anthropic notifies you of a security breach within 48 hours (Sec. G.1). The NPC's 72-hour clock can start when the processor knows, not only when you know ([IRR, Sec. 38(a)](https://privacy.gov.ph/implementing-rules-regulations-data-privacy-act-2012/)).
- Within 30 days after the agreement ends, Anthropic deletes all copies of your data. Exceptions cover legal duties, disputes, and harmful use (Sec. H.1.b).
- "Applicable Data Protection Laws" means all applicable privacy laws (Sec. A.1). The text does not name the Philippines.
- EU Standard Contractual Clauses and a UK Addendum cover transfers (Schedule 3).
- I did not compare the addendum item by item with IRR Sec. 44. Whether it meets every Philippine requirement is Not verified.

Retention for API data ([Anthropic privacy center](https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-organization-s-data), dated 1 July 2026):

- Anthropic deletes inputs and outputs "within 30 days of receipt or generation".
- Exceptions: services with longer retention under your control, such as the Files API; a zero data retention agreement; Usage Policy enforcement; and legal duties.
- If Anthropic flags a request for a Usage Policy violation, it keeps inputs and outputs up to 2 years. It keeps classification scores up to 7 years.

### Claude Max and other consumer plans

The map notes that AI usage may come from Claude Max credits. Consumer plans fall under different terms ([Consumer Terms](https://www.anthropic.com/legal/consumer-terms), effective 8 October 2025):

- You "may not make your Account available to anyone else" (Sec. 2).
- You may not access the services "through automated or non-human means" unless you use an API key or Anthropic "explicitly permit[s] it" (Sec. 3, item 7).
- Anthropic may use your inputs and outputs for training "unless you opt out". Even after an opt-out, Anthropic can use feedback and flagged content (paragraph "Our use of Materials").
- If you allow training, Anthropic may keep data in de-identified form for up to 5 years ([consumer retention article](https://privacy.claude.com/en/articles/10023548-how-long-do-you-store-my-data)).
- I found no data processing addendum in the consumer terms. Without one, you may have no processor contract to show under RA 10173 Sec. 21(a) or IRR Sec. 44.

### Google Gemini API

From the [Gemini API Additional Terms](https://ai.google.dev/gemini-api/terms), effective 23 March 2026:

- Unpaid services: Google uses prompts and responses to improve its products. Human reviewers may read them. The terms say: "Do not submit sensitive, confidential, or personal information to the Unpaid Services."
- Paid services: Google does not use prompts or responses to improve its products. It logs them for a limited period to detect misuse. The processor data terms apply.
- For apps offered to people in the EEA, Switzerland, or the UK, you may use only paid services.
- The 18+ rule in section 2 applies.

### OpenAI API

- Since 1 March 2023, OpenAI does not use API data to train models unless you opt in ([OpenAI data controls](https://developers.openai.com/api/docs/guides/your-data)).
- OpenAI keeps abuse monitoring logs for up to 30 days by default (same page).
- OpenAI uses customer content only to provide the services, unless you agree otherwise ([Services Agreement, Sec. 4.2](https://openai.com/policies/services-agreement/)).
- You warrant that you have all rights to provide your input ([Sec. 4.3](https://openai.com/policies/services-agreement/)).
- A data processing addendum applies when you process personal data ([Sec. 5.3](https://openai.com/policies/services-agreement/)).

## 5. Learners in other countries

### European Union (GDPR)

- The GDPR applies to a controller outside the EU that offers services to people in the EU. This holds "irrespective of whether a payment" is required ([GDPR, Art. 3(2)(a)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)).
- "The mere accessibility" of a website in the EU is "insufficient" to show an intention to offer services ([Recital 23](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)). The recital gives EU languages and currencies as signs of intention.
- So the GDPR depends on whether you target EU learners, not on your size.

If the GDPR applies:

- Processing that is necessary for a contract with the person is lawful ([Art. 6(1)(b)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)).
- Erasure is a right when data is no longer needed or consent is withdrawn ([Art. 17(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)).
- You may use only processors with sufficient guarantees, under a binding contract ([Art. 28(1) and 28(3)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)).
- Report a breach to the supervisory authority "not later than 72 hours" after you become aware of it ([Art. 33(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)).
- A controller under Art. 3(2) must name an EU representative ([Art. 27(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)). The exemption needs processing that is occasional, without large-scale special data, and unlikely to cause risk ([Art. 27(2)(a)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)).
- Records of processing are exempt below 250 persons. The exemption fails if processing is "not occasional" ([Art. 30(5)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)).
- The GDPR's special categories do not include education or age ([Art. 9(1)](https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679)). This differs from the Philippine list of sensitive information.

News reports describe proposed GDPR amendments, the "Digital Omnibus". I did not check their status on an official EU page. Not verified.

### United Kingdom (UK GDPR)

- Art. 3(2) of the UK GDPR applies to controllers outside the UK that offer services to people in the UK ([UK GDPR, Art. 3](https://www.legislation.gov.uk/eur/2016/679/article/3)).
- Art. 27 requires a UK representative, with the same exemption for occasional, low-risk processing ([UK GDPR, Art. 27](https://www.legislation.gov.uk/eur/2016/679/article/27)).
- The Data (Use and Access) Act 2025 amended parts of the UK GDPR in 2026 ([note on Art. 27](https://www.legislation.gov.uk/eur/2016/679/article/27)). I did not review the other changes. Not verified.
- The ICO Children's code applies as described in section 2.

### United States

- COPPA is in section 2. The DMCA is in section 3.
- I did not research US state privacy laws. Not verified.

## 6. Which duties depend on size

| Duty | Size or scale condition | Applies to a small test? |
| --- | --- | --- |
| Privacy notice, lawful basis, rights (PH) | None | Yes |
| Accountable person or data protection officer (PH, Sec. 21(b)) | None | Yes |
| Security measures (PH, Sec. 20) | Scaled by size and cost (Sec. 20(c)) | Yes, at a lower level |
| Breach notice within 72 hours (PH) | None. When unsure, you consider whether 100 or more individuals are involved (16-03, Sec. 13(B)) | Yes, when the Sec. 11 conditions hold |
| Annual incident summary (PH, 16-03, Sec. 22) | None | Yes |
| Registration (PH, 2022-04, Sec. 5) | 250 employees or 1,000 sensitive records. Profiling and likely risk apply at any size | Open: depends on the profiling and risk questions |
| Sworn declaration (PH, 2022-04, Sec. 6) | Applies when you do not register | Yes, if you do not register |
| Maximum criminal penalty (PH, Sec. 35) | 100 or more persons affected | Raises the stakes as Suro grows |
| GDPR applies at all (Art. 3(2)) | Targeting the EU, not size | Depends on your marketing |
| GDPR representative (Art. 27) | Occasional, low-risk processing is exempt | Probably required if GDPR applies |
| GDPR records (Art. 30(5)) | Under 250 persons, unless not occasional | Probably required if GDPR applies |
| COPPA | Audience and actual knowledge, not size | A neutral gate lowers the chance of actual knowledge. It does not prevent a "directed to children" finding |
| DMCA safe harbor | None. Agent designation costs USD 6 | Optional shield |
| AI provider terms | None | Yes |

## 7. What this means for Suro

These are findings and options for the v1 spec. A later ticket with the owner decides.

Duties that appear to apply even to a small test with Philippine learners:

1. Publish a privacy notice that names the AI provider as a recipient and states retention periods.
2. Choose and record a lawful basis for each purpose. If you use consent, make it granular, express, recorded, and easy to withdraw.
3. Name yourself as the accountable person, with a contact address.
4. Register with the NPC, or file the sworn declaration. The choice depends on the open profiling and risk questions.
5. Keep a written incident policy, an incident log, and a breach notice plan that meets 72 hours.
6. Build one deletion path that covers the account, materials, mastery records, and the gate record.
7. Set a retention period for each kind of data. Do not keep data "in perpetuity".
8. Use an AI access path with a processor contract and no training on inputs. The Anthropic API terms include both. I found neither in the consumer Max terms.
9. Run an adults-only gate that blocks under-18 sign-ups and stores as little as possible about age. Record why its method fits Suro's risk.
10. Ask learners to confirm they may use each material. Offer a way for rights holders to request removal.

## Not verified

- Whether stored mastery counts as "profiling" under NPC Circular 2022-04, Sec. 2(L).
- Whether mastery records count as information about "education" under RA 10173, Sec. 3(l)(2).
- Whether a yes-or-no age answer counts as information about "age" under Sec. 3(l)(1).
- Whether Suro's processing "will likely pose a risk" under Circular 2022-04, Sec. 5.
- Whether the IRR Sec. 47 "not occasional" test still applies next to Circular 2022-04.
- How a private person with no business registration registers with the NPC.
- Whether the Anthropic data processing addendum meets every item in IRR Sec. 44.
- Whether IP Code Sec. 187.1 covers a copy that a service makes for a learner.
- Whether Suro fits RA 8792 Sec. 30 or 17 U.S.C. 512(c), given that it processes materials with AI.
- Whether a free side project is "operated for commercial purposes" under COPPA.
- The status of Philippine IP Code reform and the EU Digital Omnibus.
- The contents of NPC Advisory 2024-01 (model clauses).
- What an adult-only service must do when it learns that a learner is under 18.
- The UK Data (Use and Access) Act 2025 changes beyond Art. 27.
- US state privacy laws.
- Whether a Philippine law prescribes an age-check method. My search was not exhaustive.

## References

Philippines, privacy:

- RA 10173, Data Privacy Act of 2012: <https://privacy.gov.ph/data-privacy-act/>
- Implementing Rules and Regulations: <https://privacy.gov.ph/implementing-rules-regulations-data-privacy-act-2012/>
- NPC Circular 16-03, Personal Data Breach Management: <https://privacy.gov.ph/wp-content/uploads/2022/01/sgd-npc-circular-16-03-personal-data-breach-management.pdf>
- NPC Circular 2022-04, Registration: <https://privacy.gov.ph/wp-content/uploads/2023/05/Circular-2022-04-1.pdf>
- NPC Circular 2023-04, Guidelines on Consent: <https://privacy.gov.ph/wp-content/uploads/2023/11/NPC-Circular-No.-2023-04_Guidelines-on-Consent_07Nov2023.pdf>
- NPC Advisory 2024-03, child-oriented transparency: <https://privacy.gov.ph/wp-content/uploads/2024/12/Advisory-2024.12.17-Guidelines-on-Child-Oriented-Transparency-w-SGD.pdf>
- NPC Advisory 2024-04, AI systems: <https://privacy.gov.ph/wp-content/uploads/2025/02/Advisory-2024.12.19-Guidelines-on-Artificial-Intelligence-w-SGD.pdf>
- NPC Advisory 2026-02, breach notification: <https://privacy.gov.ph/wp-content/uploads/2026/05/Advisory-Clarification-on-the-Submission-of-Personal-Data-Breach-Notification-through-DBNMS_11-May-2026.pdf>
- NPC FAQ on registration: <https://privacy.gov.ph/pips-and-pics/faqs/>
- NPC advisories and circulars list: <https://privacy.gov.ph/pips-and-pics/advisories-circulars/>

Philippines, other laws (unofficial copies):

- RA 8293, Intellectual Property Code: <https://lawphil.net/statutes/repacts/ra1997/ra_8293_1997.html>
- RA 10372, IP Code amendments: <https://lawphil.net/statutes/repacts/ra2013/ra_10372_2013.html>
- RA 8792, Electronic Commerce Act: <https://lawphil.net/statutes/repacts/ra2000/ra_8792_2000.html>
- RA 6809, age of majority: <https://lawphil.net/statutes/repacts/ra1989/ra_6809_1989.html>

EU and UK:

- GDPR, Regulation (EU) 2016/679: <https://eur-lex.europa.eu/legal-content/EN/TXT/HTML/?uri=CELEX:32016R0679>
- UK GDPR, Art. 3: <https://www.legislation.gov.uk/eur/2016/679/article/3>
- UK GDPR, Art. 27: <https://www.legislation.gov.uk/eur/2016/679/article/27>
- ICO Children's code, services covered: <https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/childrens-information/childrens-code-guidance-and-resources/age-appropriate-design-a-code-of-practice-for-online-services/services-covered-by-this-code/>

United States:

- COPPA Rule, 16 CFR Part 312: <https://www.ecfr.gov/current/title-16/chapter-I/subchapter-C/part-312>
- FTC COPPA FAQ: <https://www.ftc.gov/business-guidance/resources/complying-coppa-frequently-asked-questions>
- 17 U.S.C. 512 (unofficial copy): <https://www.law.cornell.edu/uscode/text/17/512>
- DMCA Designated Agent Directory: <https://www.copyright.gov/dmca-directory/>
- 37 CFR 201.3, fees: <https://www.ecfr.gov/current/title-37/chapter-II/subchapter-A/part-201/section-201.3>
- 37 CFR 201.38, designated agents: <https://www.ecfr.gov/current/title-37/chapter-II/subchapter-A/part-201/section-201.38>

AI providers:

- Anthropic Commercial Terms: <https://www.anthropic.com/legal/commercial-terms>
- Anthropic Data Processing Addendum: <https://www.anthropic.com/legal/data-processing-addendum>
- Anthropic Consumer Terms: <https://www.anthropic.com/legal/consumer-terms>
- Anthropic Usage Policy: <https://www.anthropic.com/legal/aup>
- Anthropic Supported Regions: <https://www.anthropic.com/supported-countries>
- Anthropic API retention: <https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-organization-s-data>
- Anthropic consumer retention: <https://privacy.claude.com/en/articles/10023548-how-long-do-you-store-my-data>
- Gemini API Additional Terms: <https://ai.google.dev/gemini-api/terms>
- OpenAI data controls: <https://developers.openai.com/api/docs/guides/your-data>
- OpenAI Services Agreement: <https://openai.com/policies/services-agreement/>
