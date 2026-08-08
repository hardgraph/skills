# Marketing your Capital program

Build marketing assets for Capital.

> Capital for platforms is available in [public preview](https://docs.stripe.com/release-phases.md).

This guide helps you create marketing assets for Capital that educate your users and promote your Capital program. Learn how to co-brand your product with Stripe and develop content that adheres to [regulatory requirements](https://docs.stripe.com/capital/regulatory-compliance.md) applicable to Capital.

Financing is a highly regulated space in every market where Capital operates. All new or updated customer-facing material referencing Stripe Capital must be reviewed and approved by Stripe and its financial partners before publication. This requirement applies throughout the entire lifespan of your Capital program.

If you plan to use Capital marketing material in multiple regions, you must obtain approval to use it in each region.

## Marketing requirements

Depending on your Capital integration type, additional platform marketing might be optional.

- **No-code integration**: For no-code integrations, Stripe sends offer emails on your behalf. While this isn’t required, we recommend that you [send an announcement email](https://docs.stripe.com/capital/marketing.md#announcement-email) and develop a marketing landing page using our [messaging guidelines](https://docs.stripe.com/capital/marketing.md#messaging).
- **Embedded components:** You can use embedded components to notify users of their offers within your product. You can either use Stripe’s no-code promotional offer emails or send your own offer notifications. You must review and comply with the [requirements for sending offer notifications](https://docs.stripe.com/capital/marketing.md#financing-notifications).
- **API integration**: For API integrations where you’re responsible for notifying users about their Capital offers, you must review and comply with the section on [sending offer notifications](https://docs.stripe.com/capital/marketing.md#financing-notifications) to build an email campaign or in-Dashboard notifications with the [Capital API](https://docs.stripe.com/capital/api-integration.md#retrieve-financing-offers).

> You must submit any customer-facing materials related to Capital to Stripe for review prior to being published using the [Compliance Intake Form](https://form.asana.com/?k=8K51UWmWhttehNFD5qBLdg&d=974470123217835).

## Marketing approval process

To comply with regulatory requirements, all marketing materials used to promote the Capital program must be reviewed, approved, and retained by Stripe and its financial partners. Also, platforms must maintain records of all such materials.

Items that must be submitted to Stripe for approval include:

- Offer materials, regardless of channel
- Email marketing, including proposed targeting populations
- Website or UX material referencing the Stripe Capital product
- Blog posts or social media referencing the Stripe Capital product
- In-app or push notifications
- Customer testimonials

This approval is not a one-time requirement. You must resubmit any update, modification, or addition to marketing materials—no matter how minor—for compliance review before using them.

Also, approval in one market doesn’t automatically extend to other markets. For example, materials approved for the US require a separate review before use in France, Germany, or Australia.

You must submit any marketing materials that you create or modify to Stripe’s compliance team using the [Compliance Intake Form](https://form.asana.com/?k=8K51UWmWhttehNFD5qBLdg&d=974470123217835) prior to use or publication. We respond by email with edits or an approval. The standard turnaround time is 5-7 business days.

If we request edits, you must implement them and resubmit the edited materials for approval before use or publication.

To receive approval, the material must align with the requirements and guidelines detailed in this guide.

When submitting your materials:

- Read and adhere to the guidelines on this page.
- Provide your platform name.
- Provide full screenshots or mockups that include headers and footers in a legible, high-resolution format.
- Submit materials as an editable Google Doc where possible; PDF is also acceptable.
- Describe the type of marketing material you’re submitting (for example, landing pages, web banners, emails, search engine marketing) and whether it includes images, text, or both. Include associated URLs if submitting web pages.
- Describe where all CTA links direct (for example, a landing page URL or login screen).
- For email marketing, include a sample of your standard email footer showing an opt-out link and physical address.

## Sending offer notifications 

As part of your Capital marketing program, you can set up offer notifications so your connected accounts know when they’re eligible for financing. You must notify all eligible connected accounts about available offers.

With the no-code option, Stripe sends co-branded offer emails on your behalf. If you’re using the API integration, you must build and deploy your own email campaign.

To get started select your integration type below.

#### No-Code

For customers in the US and Australia, Stripe Capital’s no-code email service sends a series of emails to notify them of their offers and keep them informed about payment progress. All emails are co-branded with your logo and brand colors.

The no-code email service sends the following emails:

1. **First offer email**: This email notifies the customers about their first prequalified offer. We send it as soon as a customer becomes eligible.
2. **Weekly update emails**: If a customer has active financing, we send them a weekly email updating them on the current amount paid, with a link to a Stripe-hosted active financing page.
3. **Repeat offer email**: After a customer has fully paid their financing, we send them an additional offer email as soon as they’re eligible.
4. **Followup offer emails**: If a customer doesn’t accept an offer and is still eligible, we send a followup offer email every 30 days.
5. **Additional transactional emails**: We also send other relevant transactional emails to customers, such as for application questions or financing payments.

All emails are co-branded with your logo and brand colors. To see the end-to-end user experience, including the offer email, check out the [Capital demo experience](https://dashboard.stripe.com/capital/demo). You can customize the colors and logo in the email by adjusting your [Connect settings](https://dashboard.stripe.com/settings/connect/onboarding-interface).

> #### Connect branding settings
> 
> Adjusting Connect settings changes your branding for all of Connect, not just Capital.

#### API

### Send your own offer emails 

If you want to send your own financing offer emails instead of using our no-code email services, you can do so with the [Capital API](https://docs.stripe.com/capital/api-integration.md). You must notify all eligible connected accounts of their offer.

Below are email program recommendations and sample email templates. Before sending any emails, you must submit them for review using the [Compliance Intake Form](https://form.asana.com/?k=8K51UWmWhttehNFD5qBLdg&d=974470123217835).

### Request Capital Marketing Templates

Stripe can provide Capital Marketing template materials to help you structure your offer notification campaign. Contact your Stripe account team or Stripe Support to request templates.

Stripe-provided templates are not pre-approved materials. If you use them, you must still submit all materials for approval through the [Compliance Intake Form](https://form.asana.com/?k=8K51UWmWhttehNFD5qBLdg&d=974470123217835), even if you use unmodified templates. Standard turnaround is 5–7 business days.

### Email cadence guidance

We recommend a drip campaign for first offers, with re-engagement every 30 days for eligible users who haven’t accepted. A typical program includes:

1. **First offer email**: Sent as soon as a customer becomes eligible.
2. **Reminder emails**: Sent 7 and 14 days after the first offer, if not accepted. Configure these to stop after a customer accepts.
3. **Refill offer email**: Sent when a user is [eligible for a refill](https://docs.stripe.com/capital/refills.md) (offered before current financing is completely paid, if a user is making good progress on paying existing financing).
4. **Repeat offer email**: Sent after a customer fully repays and becomes eligible again.
5. **Re-engagement email**: Sent every 30 days for customers who remain eligible but haven’t accepted.

When delivering these emails, you must comply with:

- Relevant marketing and communications requirements, including [CAN-SPAM (US)](https://docs.stripe.com/capital/regulatory-compliance.md#can-spam), [PECR (UK)](https://docs.stripe.com/capital/regulatory-compliance.md#pecr), applicable EU/EEA marketing regulations (FR, DE), and the Australian Spam Act (AU).
- Document retention requirements, including copying [capital-offers@stripe.com](mailto:capital-offers@stripe.com) on all email campaigns.
- [UDAP requirements (US)](https://docs.stripe.com/capital/regulatory-compliance.md#udap) and applicable equivalents in other markets.

### In-product marketing

In-product upsells, such as banners or dialogs, help deliver targeted marketing messages directly to your customers. You can also use Stripe’s [Capital promotional component](https://docs.stripe.com/connect/supported-embedded-components/capital-financing-promotion.md). Use the Capital API to make sure upsells only appear for eligible users and present accurate offer information. Stripe can provide template copy and UX guidance for in-product marketing placements on request. Contact your Stripe account team to request these materials.

Templates require compliance review. All in-product marketing materials, including templates provided by Stripe, must be submitted for compliance review before deployment. This applies to each market separately.

## Required disclaimers 

All marketing collateral must mention our banking partners Celtic Bank, Lead Bank, and YouLend. You must add the following disclaimer on any user-facing materials wherever you reference Stripe Capital, such as promotional offer emails and marketing pages. The disclaimer doesn’t need to be the same font size as the marketing message, but it must be clear, conspicuous, and in a font color or drop shadow that’s in contrast to its background.

The table below lists the required disclaimer based on the location of your connected account’s business. For financing offer emails sent by your platform via the Capital API or custom banner notifications, the [disclaimer_variant](https://docs.stripe.com/api/capital/connect_financing_object.md#financing_offer_object-disclaimer_variant) property identifies the required disclaimer.

| Country | Required disclaimer | [disclaimer_variant](https://docs.stripe.com/api/capital/connect_financing_object.md#financing_offer_object-disclaimer_variant) |
| --- | --- | --- |
| AU | Financing is provided by Fundbox, an external finance provider, with payments facilitated by Stripe. Funding amounts, rates, and terms are based on review and approval of a completed application, and are subject to change. Financing offers might require additional documentation. | `fundbox_au_financing` |
| DE | All financing requests are subject to final review before approval. Financing is provided in cooperation with YouLend GmbH and its affiliates. Technical support and customer management provided by YouLend GmbH.

Stripe Technology Europe Limited acts as an intermediary and technology provider. Stripe Technology Europe Limited holds a license for loan brokerage pursuant to Sect. 34c para. 1 sentence 1 no. 2 of the German Trade Ordinance. Competent Supervisory Authority: Industrie- und Handelskammer für München und Oberbayern, 80333 München. <https://www.ihk-muenchen.de/de/> | `youlend_de_financing` |
| DE | Alle Finanzierungsanfragen unterliegen einer abschließenden Prüfung vor der Genehmigung. Die Finanzierung erfolgt in Zusammenarbeit mit der YouLend GmbH und ihren verbundenen Unternehmen. Technischer Support und Kundenbetreuung werden durch die YouLend GmbH bereitgestellt.

Die Stripe Technology Europe Limited tritt als Vermittler und Technologieanbieter auf. Stripe Technology Europe Limited verfügt über eine Erlaubnis zur Darlehensvermittlung nach § 34c Abs. 1 S. 1 Nr. 2 Gewerbeordnung (GewO). Zuständige Aufsichtsbehörde: Industrie- und Handelskammer für München und Oberbayern, 80333 München. <https://www.ihk-muenchen.de/de/> | `youlend_de_financing` |
| FR | All financing requests are subject to final review prior to approval. Financing is provided in co-operation with YouLend SAS and its affiliates. Technical support and customer management provided by YouLend SAS.

YouLend SAS is registered in the Single Register of Insurance, Banking, and Finance Intermediaries ([ORIAS](https://www.orias.fr/)) under the registration number N 21001409 as an Intermediary in Banking Operations and Payment Services (MOBSPL). YouLend SAS’s registered office is located at the SNCF station, 14 rue de Dunkerque, 75010, Paris. | `youlend_fr_financing` |
| FR | Toutes les demandes de financement font l’objet d’un examen final avant approbation. Le financement est fourni en coopération avec YouLend SAS et ses sociétés affiliées. L’assistance technique et la gestion client sont assurées par YouLend SAS.

YouLend SAS est immatriculée à l’organisme pour le registre unique des intermédiaires en assurance, banque et finance ([ORIAS](https://www.orias.fr/)) sous le numéro d’immatriculation N 21001409 en tant qu’intermédiaire en opérations bancaires et services de paiement (MOBSPL). Le siège social de YouLend SAS est situé à la gare SNCF, 14 rue de Dunkerque, 75010, Paris. | `youlend_fr_financing` |
| UK | All financing applications are subject to review prior to approval. In the UK, Stripe Capital loans and merchant cash advances are provided by YouLend. | `youlend_uk_mca` |
| US | Stripe Capital offers financing types that include loans and merchant cash advances. All financing applications are subject to review prior to approval. In the US, Stripe Capital loans are issued by Celtic Bank or Lead Bank, and YouLend provides Stripe Capital merchant cash advances.

Stripe Capital loans have a minimum amount due each payment period. If the amount that you pay through sales doesn’t meet the minimum required, your bank account will be automatically debited the remaining amount at the end of the period. | `celtic_us_loan` or `youlend_us_mca` |

### Name your Capital program 

Keep the name of your Capital program simple. If you already have a payments program such as “Rocket Rides Payments," you can call it “Rocket Rides Capital” or “Rocket Rides Business Financing." If you want to include Stripe in the name, we recommend something like “Rocket Rides + Stripe Capital."

### Design your Capital Logo 

Follow this format for partnership logos

- Place your logo on the left with the Stripe logo on the right.
- Join the two logos with a `|` symbol or `+` symbol.
- Make both logos the same size. Don’t make one larger or smaller than the other.
- Use the [downloadable Stripe logo kit](https://stripe.com/newsroom/information#:~:text=The%20Stripe%20Logo). Don’t use the badge or Connect button.

## Messaging 

Refer to the following copy guidelines when you write marketing copy for Stripe Capital on any marketing assets. You can modify the wording to suit your platform’s voice, as long as it conveys the same message and uses the correct terminology as detailed below.

Before publishing, you must submit all customer-facing materials for review using the [Compliance Intake Form](https://form.asana.com/?k=8K51UWmWhttehNFD5qBLdg&d=974470123217835).

### Terminology reference 

The list of approved and prohibited language isn’t exhaustive. For any questions, contact the Capital team at [capital-review@stripe.com](mailto:capital-review@stripe.com).

| Approved messaging | Prohibited messaging |
| --- | --- |
| - Payment rate
- Pay as a fixed percentage of sales/invoices | - Interest rates
- High interest rates/high-rate
- APR |
| - Prequalified | - Preapproved |
| - Pay
- Payment
- Paid | - Repay
- Repayment
- Repaid
- Only pay when you get paid
- Use the paycheck to cover your financing |
| - Payment rate
- Pay as a fixed percentage of sales/invoices | - Interest rates
- High interest rates/high-rate
- APR
- Repayment |
| - Immediate / ongoing business expenses | - Short-term business expenses |
| - Scan to see what’s available | - Scan to view offer |
| - Pair with: “All financing applications are subject to review prior to approval.” | - No applications. No paperwork. |
| - Applying might require a personal credit check, but this won’t affect your personal credit score. | - No credit check (MCAs only) |
| - If Approved | - When Approved |
| - Financing through [platform] | - Financing from [platform] |
| - Pair with: “All financing applications are subject to review prior to approval.” | - No applications. No paperwork. |
| - These phrases must appear in close proximity to the phrase “pay your financing through a portion of your daily sales.”
  - Automatic and flexible payment
  - Pay when you get paid | - Pay back the financing at any time
- Payment on your schedule
- Pay it off in pieces
- Pay it back whenever |
| - Flat fee
- Financing fee | - Interest rates
- High interest rates |
| - Financing
- Business financing | - Low risk financial solution
- Loans |
| - Offer through Stripe Capital
- Financing through our partner, Stripe Capital
- Stripe Capital offer
- Powered through Stripe Capital | - Offer from Stripe
- Offer from [Platform]
- Powered by Stripe Capital |
| - Typically in 2 business days
- As soon as 2 business day
- Get funds fast | - In 2 business days
- Get funds the next business day
- Get funds instantly |
| - Simple and fast for you to get the financing for your business | - Get instant financing
- Access to loans/financing right away |
| - Financing for business needs | - Financing for any of your needs
- Consumer financing |
| - Applying for financing through Stripe Capital might require a personal credit check, but this credit check doesn’t affect your personal credit score
- Won’t affect your personal credit score | - No credit check
- Won’t affect your credit |
| Use business financing for:
- Creating/publishing/purchasing advertisements
- Costs of hiring new team members/recruitment costs
- Buying new equipment
- Keeping extra cash on hand
- Funding to grow your business | Use business financing for:
- Offsetting personal expenses such as groceries and home expenses
- Using funds for personal, family, or household purposes
- Paying personal bills
- No downsides |
| - [when processing payments through Stripe you] “…can begin the eligibility process”
- [the eligibility process] “…takes into account transaction amounts that you’ve demonstrated you can pay” | - [when processing payments through Stripe you] “…are automatically eligible to qualify”
- [when processing payments through Stripe you] “…won’t offer you more than you can afford/show you can pay”
- Consumer financing |
| - [when discussing eligibility] “…additional steps you can take to streamline your evaluation” | - [when discussing eligibility] “…increase the amount of money you’ll be eligible to borrow” |
| - Stripe’s easy/straightforward/integrated process | - Stripe’s responsible lending practices
- “ensures responsible financing practices”
- Any language implying that underwriting practices ensure no chance of default |

## Promote your Capital program 

When your Capital program goes live, you’ll want to share information about it. Because not all of your customers will be eligible for Capital, make sure that any broader marketing, such as a landing page, doesn’t promise eligibility. Instead, you can use marketing to explain how the program works, why it’s important, and where customers can check their eligibility.

Review our messaging guide for help crafting your marketing materials. We also recommend referencing your broader vision for embedded finance if you’re also using Stripe Treasury or Stripe Issuing.

Before publishing, you must submit all customer-facing materials for review using the [Change Request Form](https://form.asana.com/?k=8K51UWmWhttehNFD5qBLdg&d=974470123217835).

### Announcement email 

Whether you’re building a stand alone email or adding copy to an existing newsletter, emails are a great way to introduce Capital to your customer base. Try to keep your email short and to the point, but make sure to include these key points:

- A brief introduction to the program
- Where customers will learn about their offer (make sure to touch on eligibility since not all visitors to your page will be eligible)
- Link to a blog post, landing page, or place where they can check their eligibility
- Compliance disclosures
- Opt out/unsubscribe ability so that the customer may exercise their right to not receive Capital marketing or be eligible to receive an offer

Refer to our messaging guide for help crafting your email copy. Refer to our [regulatory compliance guide](https://docs.stripe.com/capital/regulatory-compliance.md) to check that your marketing adheres to the relevant guidelines.

### Landing pages and blog posts 

Evergreen content like landing pages and blog posts can be a great way to excite prospective customers about your financing program, and educate existing customers about how it works. The following sections provide tips for developing your content. We also recommend reviewing the messaging guide as you have some additional things to consider when creating this type of marketing content:

- Comparisons: Due to the additional regulatory complexity of comparing Stripe Capital against other competitors or different loan products, you may not directly compare Stripe Capital against any competitors’ offerings.
- Testimonials: Writing about your customer’s experiences in a blog post related to Stripe Capital falls under “testimonials” rules and must adhere to specific requirements.
- Substantiating claims: Blog posts sometimes make large claims about overall successes, such as “Stripe Capital helps our users double their output.” Make sure that you have accurate and verifiable data for any quantifiable claims. You must validate these claims and provide supporting evidence upon request. Every claim must be substantiated and readily available for the duration the content is active, and for up to five years following its last use.

**Suggested page structure**

1. Brief introduction to the program
2. Why financing can help their business
3. How the program works
4. Where customers will learn about their offer (make sure to touch on eligibility since not all visitors to your page will be eligible)
5. Links to FAQs, docs or any other pages you’d created
6. Compliance disclosures (default disclaimer)

#### Landing page URL and site navigation 

Here are a few examples for landing page URL options and site navigation categories. You can also use the name of your program such as “[Platform name] Capital” as the navigation category.

- `/capital`
- `/financing`
- `/business-financing`

## Marketing to ineligible users

Not all customers are eligible for a Capital offer. Marketing available to both eligible and ineligible customers must clearly explain how eligibility is determined. Don’t market Capital offers directly to confirmed ineligible recipients.

## See also

- [Servicing](https://docs.stripe.com/capital/servicing.md)
- [Metrics](https://docs.stripe.com/capital/reporting.md)
