# How Stripe Capital for platforms works

Learn the basics about Stripe Capital for platforms.

> Capital for platforms is available in [public preview](https://docs.stripe.com/release-phases.md).

Stripe Capital for platforms is available to *Connect* (Connect is Stripe's solution for multi-party businesses, such as marketplace or software platforms, to route payments between sellers, customers, and other recipients) platforms with eligible connected accounts in the following countries:

- AU
- DE
- FR
- GB
- US

Eligibility requirements, financing processes, and offer structures are largely consistent across markets. Find country-specific details and offer types in the tabs below.

## Eligible connected accounts

Stripe, alongside our financial partners, reviews each connected account’s eligibility based on their payment activity on Stripe. This automatic review runs daily and requires no action from your platform. While we aim to offer financing to as many accounts as possible, not all connected accounts are eligible.

### Minimum eligibility requirements

To qualify, a connected account must:

- Be a for-profit business
- Be located or incorporated in one of the capital supported countries and be able to provide an address in that country
- Have processed payments on Stripe with the platform for at least 3 months
- Have a processing volume of at least 5,000 in local currency per year, and an average of 1,000 in local currency over the last 3 months
- Be in good standing with Stripe Capital
- The person applying and guaranteeing the offer must be a representative, controller, or director with at least 25% ownership and must be at least 18 years old

Meeting the minimum eligibility requirements doesn’t guarantee that a connected account qualifies for a Stripe Capital offer.

In addition to the minimum requirements, Stripe, along with our partners, evaluates many other characteristics of the connected account’s business to determine eligibility. Key considerations include:

- **A steady processing record**: A consistent, steady processing record with limited periods of low or zero volume shows stability in the business and increases their likelihood of qualifying for an offer.
- **A growth trajectory**: The amount of payment they process through Stripe influences the size of a connected account’s funding offer. Businesses with positive growth trajectories are more likely to be eligible for an offer.
- **A large customer base**: Businesses with more customers are more likely to be eligible for an offer.
- **A low dispute rate**: Businesses with low rates of unresolved *chargebacks* (The action taken by a cardholder's bank to debit a business's account in response to a dispute from the cardholder. The debited funds are held until the dispute is resolved) are more likely to qualify for funding.

## Financing process

To provide a connected account a financing offer, Stripe Capital follows a financing process:

| **Phase** | **Description** |
| --- | --- |
| **Risk and underwriting** | Stripe prequalifies each connected account that receives an offer. Stripe uses a combination of eligibility criteria (including overall processing volume and history on Stripe with your platform) to extend financing offers for connected accounts. |
| **Promote Capital offers** | Show financing offers to eligible connected accounts through embedded components or email. Co-branding options vary by market. See [Compare integrations](https://docs.stripe.com/capital/getting-started.md#compare-integrations) for country-specific guidance. Regardless of who sends offer emails, connected accounts must accept their offer in a Stripe-hosted Capital page or an embedded component. |
| **Fund disbursement** | Stripe handles sourcing all Capital funds and deposits them to your connected account’s bank account within 1–2 business days. |
| **Payments towards a financing balance** | Stripe fully automates collecting payments toward a financing balance and adjusts with daily sales. Stripe deducts a fixed percentage from each of your connected account’s sales until they pay the financing amount and fee. |
| **Servicing and collections** | Stripe and our financial partners handle questions about financing offers and manage payment collection with any connected accounts who fail to make payments toward their financing balance. |

## Capital offer structure 

Each Capital financing offer consists of the following components:

| **Component** | **Definition** |
| --- | --- |
| Principal amount | The amount the connected account is prequalified to receive |
| Payment rate | The percentage of each future transaction withheld toward payment |
| Premium amount | A flat fee on top of the principal amount that must be repaid |
| Minimum payment | A minimum amount that must be paid over a specified time period |

For example, consider a financing offer of 20,000 in local currency at a 15% payment rate with a 2,000 flat fee. After the connected account reviews and accepts the offer, they receive a 20,000 payout. Stripe withholds 15% of each transaction processed through Stripe until they pay the full balance of 22,000. In markets where a minimum payment applies, if the connected account misses a minimum payment, Stripe automatically debits the shortfall for that period.

## Payment 

Payment is fully automated. Stripe deducts a fixed percentage of each connected account’s transactions until the complete balance is repaid. There’s no prepayment penalty; connected accounts can make additional payments or pay their balance in full at any time from the Capital tab in their Dashboard.

For financing types with minimum payment requirements, if a connected account’s sales withholdings don’t meet the minimum for a given period, Stripe automatically debits the shortfall from their linked bank account. Connected accounts can also make manual payments toward their minimum from the Dashboard. If a connected account is unable to meet their minimum for one or more periods, direct them to contact support to get back on track.

Stripe automatically re-evaluates connected accounts for new offers as they pay down their financing. Paying off early doesn’t guarantee a new offer — when a connected account becomes eligible, the offer appears automatically in their Dashboard.

## Additional financing 

Refills are additional financing offers extended to connected accounts that have made substantial payment progress on an in-progress financing balance. Refill behavior varies by country. Find country-specific details and offer types in the tabs below.

## Capital integration options 

Available integration options vary by market. See [Compare integrations](https://docs.stripe.com/capital/getting-started.md#compare-integrations) to confirm which options are available in the countries of your connected accounts. There are three ways to set up your Capital program:

- [No-code integration](https://docs.stripe.com/capital/no-code-integration.md)
- [Embedded components integration](https://docs.stripe.com/capital/embedded-component-integration.md)
- [API integration](https://docs.stripe.com/capital/api-integration.md)

### Provide financing offers

To show financing offers to eligible connected accounts, use an embedded component or email. In some countries, Stripe can send co-branded marketing emails with financing offers to your connected accounts on your behalf. In others, local regulations (see [Regulatory compliance](https://docs.stripe.com/capital/regulatory-compliance.md) to learn more about market-specific regulations) require you to listen to webhooks for eligible offers and send your own notifications.

If you use an API integration, you’re responsible for updating offers as delivered and accepted. To learn more, see [Set up an API integration](https://docs.stripe.com/capital/api-integration.md).

Regardless of your integration type, connected accounts must view and accept their financing offer in a Stripe-hosted Capital page or an embedded component.
![anatomy offer](https://b.stripecdn.com/docs-statics-srv/assets/offer-anatomy.25435a5c27bd4804965991bf4ba77e00.png)

Eligible connected accounts see this Stripe-hosted Capital page or interact with this page within the embedded component to accept a financing offer, regardless of your integration type.

To learn how to launch a Capital program, see [Set up Capital](https://docs.stripe.com/capital/getting-started.md).

## Capital emails 

Stripe can send the following types of emails to your connected accounts throughout the financing process:

- **Marketing**: Co-branded emails notifying eligible connected accounts of financing offers. Available in select markets only. Includes new offers (sent when a connected account first becomes eligible), refreshed offers (sent when a prior offer expires after 30 days and the account re-qualifies), and refill offers (sent when a connected account is approximately 80% through paying their current financing and is eligible for new funding before completing their paydown). See [Capital refills](https://docs.stripe.com/capital/refills.md).

- **Application status**: Updates across the full application lifecycle, including confirmation that an application has been submitted and is under review, approval notifications with disbursement details, rejection notifications, and requests to resolve identity or business verification (KYC or KYB) issues if we can’t verify the information provided during the application.

- **Transactional**: Ongoing weekly updates throughout the payment period, including milestone progress notifications at 25%, 50%, and 75% of payment completion, reminders about outstanding minimum balance payments, and notifications about upcoming bank account debits.

## In-progress financing 

After a connected account accepts an offer and receives their payout, you can provide them with visibility into their financing balance, transaction history, and payment progress.

- **Email**: Connected accounts receive co-branded email notifications on offer acceptance, payout disbursement, and weekly or periodic payment progress. Each email links to a Stripe-hosted Capital page where they can view transactions and payment progress. These are transactional emails, enabled by default in all markets.

- **Embedded component**: Embed the [Capital financing component](https://docs.stripe.com/connect/supported-embedded-components/capital-financing.md) in your platform’s UI to provide transaction reports directly to connected accounts. This component doesn’t include standard payment processing information such as charges, refunds, or disputes.

- **API**: Use the [Financing Transactions API](https://docs.stripe.com/capital/reporting-and-reconciliation.md) to monitor Capital transactions alongside detailed payments data—including charges, refunds, and disputes—and build your own custom reporting interfaces.

For full implementation details, see [Reporting and reconciliation](https://docs.stripe.com/capital/reporting-and-reconciliation.md).

## Capital servicing 

Stripe and our financial partners handle support and servicing for your connected accounts for all Capital-related inquiries. Direct your connected accounts to [email our dedicated support team](mailto:capital+support@stripe.com) as their first point of contact. To help connected accounts facing business challenges, the Stripe team might put your connected accounts directly in touch with our financial partners who might offer extended payment plans to help them meet their financing obligations with smaller payments.

## Country specific details

#### US

### Additional eligibility considerations

- Geographic restrictions might apply and Stripe Capital might not be available to connected accounts in all US states and territories.

### Types of financing offers

Four types of financing offers are available to US connected accounts:

- **Flex Loans**: Flex term loans are made available by Celtic Bank or Lead Bank to eligible businesses. These loans have a maximum term and periodic payments. If withholdings from payment processing receivables don’t meet a minimum amount (typically on a 30 or 60-day basis), Stripe debits any shortfall from the connected account’s external bank account.
- **Merchant Cash Advance (MCA)**: MCAs are a purchase of future receivables—not a loan or credit transaction. YouLend purchases a percentage of the connected account’s payment processing volume as specified in the YouLend Advance Agreement. The MCA has no fixed payment schedule or regular debits; payments vary based on the connected account’s processing volume. Applying for an MCA might require a personal credit check, but this won’t affect the connected account’s personal credit score.
- **Fixed term loan**: Celtic Bank or Lead Bank provide fixed term loans to connected accounts that process payments off Stripe, whose payments data you [imported to Stripe](https://docs.stripe.com/capital/import-non-stripe-data.md). A fixed term loan has weekly payment minimums with a capped withholding structure.
- **Line of credit**: With a line of credit, a connected account can borrow up to a prequalified credit limit through a series of loans. They apply to draw the exact amount they need within their available limit and can access the remaining capital for 90 days. As repayments are made, the available credit replenishes, allowing them to request more capital. After 90 days, connected accounts are reevaluated for their credit limit. Each draw is reviewed and approved as a separate loan. They repay each draw as principal plus a fixed fee in nine monthly payments.

The following table compares these financing options:

| **Offer terms** | **MCA** | **Flex loan** | **Fixed term loan** | **Line of credit** |
| --- | --- | --- | --- | --- |
| **Max offer** | 100,000 USD | 250,000 USD (up to 350,000 USD for refills) | 250,000 USD | Up to prequalified credit limit |
| **Expected duration** | 5–8 months | 8–10 months | 42 weeks | 9 monthly payments per draw; limit reevaluated every 90 days |
| **Minimum payment** | None | Every 60 days | Every 7 days | Monthly per draw |
| **Credit check** | Soft pull; no impact to score | No personal credit check | No personal credit check | No personal credit check |
| **Transaction withholding** | Fixed % per YouLend Advance Agreement | Fixed % for the life of the loan | 100% of payments until the minimum is met each 7-day period; 0% thereafter | Fixed % for each active draw |

### Line of credit

With a line of credit, a connected account has a prequalified credit limit and can apply to draw the exact amount they need. For example, if a connected account has a 50,000 USD credit limit and draws 20,000 USD, they repay the 20,000 USD principal plus a fixed fee in nine monthly payments. As they make repayments, their available credit replenishes, which allows the account to request additional draws up to their available limit. Each draw is a separate loan subject to review and approval.

### Interested in offering line of credit financing to your connected accounts?

Enter your email to request access.

```bash
curl https://docs.stripe.com/preview/register \
  -X POST \
  -H "Content-Type: application/json" \
  -H "Referer: https://docs.stripe.com/capital/how-capital-for-platforms-works" \
  -d '{"email": "EMAIL", "preview": "capital_review_preview"}'
```

*Stripe Capital offers financing types that include loans and merchant cash advances. All financing applications are subject to review prior to approval. In the US, Stripe Capital loans are issued by Celtic Bank or Lead Bank, and YouLend provides Stripe Capital merchant cash advances.*

#### AU

### Additional eligibility considerations

- The person applying and guaranteeing the offer must be a representative, controller, or director with at least 25% ownership, be at least 18 years old, and be able to provide an address in Australia.
- Applicants who are directors and beneficial owners of the business must provide a personal guarantee as part of the financing application.
- Capital isn’t available to connected accounts based in New Zealand.

### Types of financing offers

Fundbox provides Australian connected accounts a single financing type: a `loan`.

| **Offer terms** | **Loan** |
| --- | --- |
| **Min offer** | 500 AUD |
| **Max offer** | 150,000 AUD (up to 250,000 AUD for refills) |
| **Expected duration** | 8–9 months |
| **Minimum payment** | Every 30 days |
| **Bank debit if minimum not met** | Yes |
| **Credit check** | No |
| **Transaction withholding** | Fixed % for the life of the loan |

### Refill behavior

When an Australian connected account accepts a refill, the payout automatically pays down the remaining balance on their current financing before the new financing begins. Connected accounts pay one offer at a time.

*Financing is provided by Fundbox, an external finance provider, with payments facilitated by Stripe. Funding amounts, rates, and terms are based on review and approval of a completed application, and are subject to change. Financing offers might require additional documentation.*

#### DE

### Regulatory requirement for platforms

In Germany, credit brokerage is a regulated activity under the German Trade Ordinance (§34c GewO). Platforms offering Capital to German connected accounts might need to obtain a credit broker license from their local Chamber of Industry and Commerce (*Industrie- und Handelskammer*) before going live. Licensing typically takes 1–3 months and generally requires a physical presence in the relevant German district. This information is for general guidance only and doesn’t constitute legal advice—seek independent legal counsel to assess the requirements that apply to your business. See [Regulatory compliance](https://docs.stripe.com/capital/regulatory-compliance.md) for more details.

### Additional eligibility considerations

- The person applying and guaranteeing the offer must be a representative, controller, or director with at least 25% ownership, be at least 18 years old, and be able to provide an address in Germany.
- Applicants who are directors and beneficial owners of the business must provide a personal guarantee as part of the financing application.

### Types of financing offers

German connected accounts can access a single financing type: a `loan advance`, provided by YouLend GmbH.

| **Offer terms** | **Loan advance** |
| --- | --- |
| **Min offer** | — |
| **Max offer** | 150,000 EUR |
| **Expected duration** | 6–7 months |
| **Minimum payment** | Weekly (contractual) |
| **Bank debit if minimum not met** | No |
| **Credit check** | Soft check on application (no score impact); hard check upon funding |
| **Transaction withholding** | Fixed % per YouLend agreement |

### Refill behavior

When a German connected account accepts a refill, the original financing remains active and the refill payout doesn’t automatically pay it. YouLend applies the new withholding rate to the original financing. After the original financing clears, withholding shifts to the new financing.

*All financing requests are subject to final review before approval. Financing is provided in cooperation with YouLend GmbH and its affiliates. Technical support and customer management provided by YouLend GmbH.* *Stripe Technology Europe Limited acts as an intermediary and technology provider. Stripe Technology Europe Limited holds a license for loan brokerage pursuant to Sect. 34c para. 1 sentence 1 no. 2 of the German Trade Ordinance. Competent Supervisory Authority: Industrie- und Handelskammer für München und Oberbayern, 80333 München. https://www.ihk-muenchen.de/de/*

#### FR

### Regulatory requirement for platforms

In France, introducing users to financing products is a regulated activity. Platforms offering Capital to French connected accounts might need to register as an IOBSP (*Intermédiaire en Opérations de Banque et Services de Paiement*) with [ORIAS](https://www.orias.fr/) before going live. Registration typically takes 2–3 months and requires a French legal entity. Stripe can assist with certain documentation as part of this process. This information is for general guidance only and doesn’t constitute legal advice—seek independent legal counsel to assess the requirements that apply to your business. See [Regulatory compliance](https://docs.stripe.com/capital/regulatory-compliance.md) for more details.

### Additional eligibility considerations

- The person applying and guaranteeing the offer must be a representative, controller, or director with at least 25% ownership, be at least 18 years old, and be able to provide an address in France.
- Applicants who are directors and beneficial owners of the business must provide a personal guarantee as part of the financing application.

### Types of financing offers

French connected accounts can access a single financing type: a `loan advance`, provided by YouLend SAS.

| **Offer terms** | **Loan advance** |
| --- | --- |
| **Min offer** | 500 EUR |
| **Max offer** | 150,000 EUR |
| **Expected duration** | 6–7 months |
| **Minimum payment** | Weekly (contractual) |
| **Bank debit if minimum not met** | No |
| **Credit check** | No |
| **Transaction withholding** | Fixed % per YouLend agreement |

### Refill behavior

When a French connected account accepts a refill, the original financing remains active and the refill payout doesn’t automatically pay it. YouLend applies the new withholding rate to the original financing. After the original financing clears, withholding shifts to the new financing.

*All financing requests are subject to final review prior to approval. Financing is provided in co-operation with YouLend SAS and its affiliates. Technical support and customer management provided by YouLend SAS.*

*YouLend SAS is registered in the Single Register of Insurance, Banking, and Finance Intermediaries (ORIAS) under the registration number N 21001409 as an Intermediary in Banking Operations and Payment Services (MOBSPL). YouLend SAS’s registered office is located at the SNCF station, 14 rue de Dunkerque, 75010, Paris.*

#### GB

### Additional eligibility considerations

- The person applying and guaranteeing the offer must be a representative, controller, or director with at least 25% ownership, be at least 18 years old, and be able to provide a UK address.
- Applicants who are directors and beneficial owners of the business must provide a personal guarantee as part of the financing application.

### Types of financing offers

There are two types of financing offers available to UK connected accounts. The connected account’s legal entity structure determines the offer type.

- **Cash advance**: A purchase of the connected account’s future receivables, not a loan or credit transaction. YouLend purchases a percentage of the business’s payment processing volume as specified in the YouLend Advance Agreement. It has no fixed payment schedule or regular debits; payments vary based on the connected account’s processing volume. It’s available to sole proprietors, individuals, and unincorporated entities.

- **Loan advance**: A loan disbursement with conditions including rates and a fixed repayment structure. It’s available to incorporated entities (companies, LLCs, Ltd).

The connected account’s current incorporation status determines the offer type and doesn’t change unless the connected account updates their incorporation status. If a connected account applies with incorrect information, ask the connected account to update their Stripe account information before reapplying.

The following table compares these financing options:

| **Offer terms** | **Cash advance** | **Loan advance** |
| --- | --- | --- |
| **Max offer** | 350,000 GBP (up to 500,000 GBP for refills) | 350,000 GBP (up to 500,000 GBP for refills) |
| **Expected duration** | 6–7 months | 6–7 months |
| **Minimum payment** | None | Weekly (contractual) |
| **Bank debit if minimum not met** | No | No |
| **Credit check** | Soft check on application (no score impact); hard check upon funding | None |
| **Transaction withholding** | Fixed % per YouLend agreement | Fixed % per YouLend agreement |

### Refill behavior

When a British connected account accepts a refill, the original financing remains active and the refill payout doesn’t automatically pay it. YouLend applies the new withholding rate to the original financing. After the original financing clears, withholding shifts to the new financing.

*All financing applications are subject to review prior to approval. In the UK, Stripe Capital loans and cash advances are provided by YouLend.*

## See also

- [Set up Capital](https://docs.stripe.com/capital/getting-started.md)
