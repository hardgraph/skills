# Treasury for platforms requirements

Understand the requirements for using Treasury for platforms.

> #### Legacy integration
> 
> The v1 version of Treasury for platforms is a legacy integration that doesn’t support many of the features introduced in [Treasury for platforms v2](https://docs.stripe.com/treasury/connect/v2.md). Don’t build a new v1 integration.

*Stripe Treasury for platforms* (A collection of API endpoints and cloud- and web-based features that enable platforms to offer embedded financial solutions to their users) have compliance requirements and restrictions in addition to those needed for [Stripe Payments](https://docs.stripe.com/payments.md). They apply to both the platform and its connected accounts. Offer your Stripe Treasury-based financial services only to businesses that meet the requirements.

## Fraud management process

Stripe requires that Treasury platforms maintain a fraud risk management process. The process must include proactive measures to prevent and monitor unauthorized or fraudulent activity, and take steps to remediate it. This includes:

- Establishing processes to identify and understand the business of your customers.
- Implementing tools to prevent unauthorized access to customer accounts.
- Educating connected accounts on how fraud can occur, and establishing a process for alerting connected accounts when suspected fraud occurs.
- Conducting root cause analyses after fraud incidents occur to identify opportunities to improve controls.

See the [Treasury for platforms Fraud guide](https://docs.stripe.com/treasury/connect/examples/fraud-guide.md) for best practices on implementing and maintaining a fraud risk management program.

## Supported countries

Treasury for platforms is available only to platforms and connected accounts located in the United States. To be considered a US business, you must be able to demonstrate:

- Your business physically operates from an address within the United States and that address can’t be a PO box, highway contract box, or private mailbox.
- Your business has at least one account representative that lives at an address within the United States. That address can’t be a PO box, highway contract box, or private mailbox.

Business owners that reside in certain countries outside of the US are permissible, but might require enhanced review. Additionally, you can’t use the Financial Account to send or receive funds to or from sources outside the United States. Stripe prohibits using Treasury for any dealings, engagement, or sale of goods or services linked directly or indirectly with jurisdictions Stripe has deemed high risk, such as Cuba, Iran, North Korea, Syria, and the Crimea, Donetsk, and Luhansk regions.

## Business use cases only

Treasury for platforms is available only to platforms with B2B use cases. Stripe doesn’t offer financial accounts to consumers or provide Treasury for platforms features for consumer purposes.

## Connected account types

Treasury for platforms only supports connected accounts that don’t use a Stripe-hosted dashboard and where your platform is responsible for requirements collection and loss liability, including Custom connected accounts. Learn how to [create connected accounts](https://docs.stripe.com/connect/interactive-platform-guide.md?connect-charge-type=direct&connect-loss-liability-owner=platform) that work with Treasury for platforms.

As a platform with connected accounts, you’re responsible for maintaining a minimum API version, communicating terms of service updates to your connected accounts, handling information requests from them, and providing them with support. Because your platform is ultimately responsible for the losses your connected accounts incur, you’re also responsible for vetting them for fraud. To learn more, read the [Treasury for platforms fraud guide](https://docs.stripe.com/treasury/connect/examples/fraud-guide.md).

## Supported countries of residence

Treasury for platforms currently supports businesses in the US only, but the business owners and authorized persons of those businesses can reside in over 150 countries. Some countries of residence, however, can require a more detailed review before you can onboard them to your platform.

Stripe prohibits using Treasury for platforms for any dealings, engagement, or sale of goods or services linked directly or indirectly with jurisdictions Stripe has deemed high risk, such as Cuba, Iran, North Korea, Syria, and the Crimea, Donetsk, and Luhansk regions.

## Prohibited and restricted business types

The businesses and business practices in the following categories are either restricted or prohibited from using Treasury for platforms. Financial network rules, the requirements of our financial services providers, and our own compliance and legal obligations determine whether Stripe can provide financial services. In some cases, a business in one of these categories can use Treasury for platforms after getting explicit approval from Stripe. The type of businesses listed below are representative of our Restricted Businesses, but this list isn’t exhaustive. The specifics might vary depending on the financial service provider.

> Businesses that offer illegal products or services are never eligible to use Treasury for platforms.

For more information about businesses restricted by Stripe Payments, and by extension Treasury for platforms, see [Prohibited and Restricted Businesses](https://stripe.com/restricted-businesses).

### Prohibited business types

The businesses and business practices in the following categories are classified as prohibited and are therefore not eligible to use Treasury for platforms:

- Adult industry, escort, or dating services
- Arms trading—retail or manufacturing
- ATMs
- Bail bonds
- Casinos, gambling or gaming
- Debt collection, debt relief, and credit restoration agencies
- Door-to-door sales
- Government agencies and entities
- Hemp or marijuana direct businesses
- Illegal drug products and services
- Money services and currency exchange
- Payable Through Accounts
- Payday lending and tax anticipation programs
- Person-to-Person payment businesses
- Shell Corporations
- Telecommunication or surveillance equipment providers
- Unfair, predatory, or deceptive practices, including multi-level marketing and pyramid schemes
- Unregistered charities
- Warranties and lifetime guarantees

### Restricted business types

The following categories of businesses and business practices are classified as restricted and are subject to enhanced review:

#### Regulated industries, such as:

- Insurance services
- Investment, commodities, and brokerage services
- Lending and cash advance services
- Money Services Businesses (MSB) and currency exchange
- Pharmaceuticals
- Student loan assistance companies
- Tobacco

#### Businesses that can pose elevated risk, such as:

- Buy Now Pay Later (BNPL)
- Direct marketing businesses, including telemarketing, “As Seen on TV”
- Import, export, and freight transport of physical commodities
- Jewelry, gems, precious metals—dealers or wholesalers
- High interest rate lenders
- Marketplaces
- Multi-level marketing
- Non-Government Organizations (NGOs)
- Non-fungible Tokens (NFTs)
- Professional Service Providers (PSP), including lawyers and accountants
- Secondhand shops and pawnshops
- Sweepstakes parlors and internet sweepstakes cafes
- Third Party Payment Processors (TPPPs)
- Vitamin, and supplement sales
- Virtual Asset Service Providers (VASPs)

## Prohibited business addresses

Treasury for platforms only supports connected accounts with a valid US-based physical business. The address can’t be:

- Of or provided by a registered agent
- A PO box
- A highway contract (HC) box
- A private mailbox

Changes to business addresses might take up to 24 business hours to be reflected on outgoing and incoming wires.

## Politically exposed persons

Stripe screens applications to identify any connected account user that’s a politically exposed person (PEP) including foreign senior political figures (SPF). Businesses that are owned by PEPs are prohibited from using Treasury for platforms services.
