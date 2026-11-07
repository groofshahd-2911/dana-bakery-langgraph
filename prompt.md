# Dana Bakery — Final Technical Specification

## Environment
Use Python, LangGraph, and Google Colab. All business logic is deterministic, with no LLM calls, model-backed agents, API keys, randomness, or live external data. Keep code beginner-friendly with descriptive names and explanatory comments.

## Inputs and catalog
An order contains item, quantity, day, and city. Normalize item and English weekday strings by stripping whitespace and lowercasing. Quantity must be a positive Python integer; reject booleans, floats, strings, and missing values. Require a valid English weekday and a string city. Reject malformed or missing fields.

| Product | Price in NIS | Stock |
| --- | ---: | ---: |
| challah | 18 | 10 |
| babka | 45 | 0 |
| rugelach | 30 | 3 |
| sourdough | 25 | 4 |

Croissant and croissants are discontinued. Reject unknown products. Each order uses the same independent stock snapshot; orders do not consume shared stock.

## Inventory and alternatives
Require sufficient inventory for the entire quantity. If the requested item is unavailable, try at most the next two products after it in babka -> rugelach -> sourdough -> challah, without wrapping. Stop on the first sufficient candidate and charge that product's price. Never split quantities. Babka quantity 4 selects sourdough on the second attempt; babka quantity 5 rejects after two attempts even though challah has stock. Challah has no subsequent alternatives.

## Pricing and delivery
Use Decimal currency arithmetic rounded to two places. Discount the baked-goods subtotal by 15% only if it is strictly greater than 150 NIS and the day is not Friday. Never discount delivery. Jerusalem (case-insensitive) delivery costs 20 NIS. Empty city means free pickup. Any other city becomes free pickup and the customer must be informed.

## Graph and results
Use TypedDict state, nodes, conditional edges, and an Annotated list reducer with operator.add for ordered steps. Route START -> validate -> inventory -> alternatives if necessary -> price -> delivery -> message -> END. Invalid input and unavailable inventory route through reject -> END. Each alternatives visit checks one candidate, with at most two visits.

Return status, item, delivery, total, message, and steps for every order. Status is accepted or rejected. Item is the fulfilled product, or normalized requested item on rejection. The delivery output must be exactly "pickup" or "delivery". Accepted Jerusalem orders use "delivery"; accepted empty-city and unsupported-city orders use "pickup". Rejected orders also use "pickup" as a valid schema default only: no fulfillment is arranged, status is "rejected", and message explicitly says the order was rejected. Keep detailed delivery location, fees, and pickup-conversion explanations in message. Total is numeric NIS and zero for rejection. Friendly deterministic messages explain substitutions, discounts, pickup conversion, and rejection.

## Notebook and verification
Include installation, explanations, state, nodes, graph construction, process_order helper, draw_mermaid_png() visualization, and at least six executable test orders with assertions covering every route and rejection. Assert that every delivery value belongs to {"pickup", "delivery"}, that rejected orders use "pickup" even for a Jerusalem request, and that message retains detailed fulfillment explanations. Include strict discount-boundary unit checks because no catalog item with available stock totals exactly 150 NIS. The optional default Mermaid renderer may use internet access without API keys; business processing stays local.

Save all notebook cells unexecuted, with null execution counts and empty outputs. Do not run, commit, or push anything during creation.
