## LED for summarization of legal documents
This is a Longformer Encoder Decoder ([led-base-16384](https://huggingface.co/allenai/led-base-16384)) model for the **legal domain**, trained for **long document abstractive summarization** task. The length of the document can be upto 16,384 tokens.

## Training data

The **legal-led-base-16384** model was trained on [sec-litigation-releases](https://www.sec.gov/litigation/litreleases.htm) dataset consisting more than 2700 litigation releases and complaints.

## Usage
```Python
from transformers import AutoTokenizer, AutoModelForSeq2SeqLM

tokenizer = AutoTokenizer.from_pretrained("nsi319/legal-led-base-16384")  
model = AutoModelForSeq2SeqLM.from_pretrained("nsi319/legal-led-base-16384")

padding = "max_length" 

text="""On March 2, 2018, the Securities and Exchange Commission announced securities fraud charges against a U.K.-based broker-dealer and its investment manager in connection with manipulative trading in the securities of HD View 360 Inc., a U.S.-based microcap issuer.  The SEC also announced charges against HD View's CEO, another individual, and three entities they control for manipulating HD View's securities as well as the securities of another microcap issuer, West Coast Ventures Group Corp.  The SEC further announced the institution of an order suspending trading in the securities of HD View.These charges arise in part from an undercover operation by the Federal Bureau of Investigation, which also resulted in related criminal prosecutions against these defendants by the Office of the United States Attorney for the Eastern District of New York.In a complaint filed in the U.S. District Court for the Eastern District of New York, the SEC alleges that Beaufort Securities Ltd. and Peter Kyriacou, an investment manager at Beaufort, manipulated the market for HD View's common stock.  The scheme involved an undercover FBI agent who described his business as manipulating U.S. stocks through pump-and-dump schemes.  Kyriacou and the agent discussed depositing large blocks of microcap stock in Beaufort accounts, driving up the price of the stock through promotions, manipulating the stock's price and volume through matched trades, and then selling the shares for a large profit.The SEC's complaint against Beaufort and Kyriacou alleges that they:opened brokerage accounts for the undercover agent in the names of nominees in order to conceal his identity and his connection to the anticipated trading activity in the accounts suggested that the undercover agent could create the false appearance that HD View's stock was liquid in advance of a pump-and-dump by "gam[ing] the market" through matched trades executed multiple purchase orders of HD View shares with the understanding that Beaufort's client had arranged for an associate to simultaneously offer an equivalent number of shares at the same priceA second complaint filed by the SEC in the U.S. District Court for the Eastern District of New York alleges that in a series of recorded telephone conversations with the undercover agent, HD View CEO Dennis Mancino and William T. Hirschy agreed to manipulate HD View's common stock by using the agent's network of brokers to generate fraudulent retail demand for the stock in exchange for a kickback from the trading proceeds.  According to the complaint, the three men agreed that Mancino and Hirschy would manipulate HD View stock to a higher price before using the agent's brokers to liquidate their positions at an artificially inflated price.  The SEC's complaint also alleges that Mancino and Hirschy executed a "test trade" on Jan. 31, 2018, coordinated by the agent, consisting of a sell order placed by the defendants filled by an opposing purchase order placed by a broker into an account at Beaufort.  Unbeknownst to Mancino and Hirschy, the Beaufort account used for this trade was a nominal account that was opened and funded by the agent.  The SEC's complaint also alleges that, prior to their contact with the undercover agent, Mancino and Hirschy manipulated the market for HD View and for West Coast by using brokerage accounts that they owned, controlled, or were associated with –including TJM Investments Inc., DJK Investments 10 Inc., WT Consulting Group LLC – to effect manipulative "matched trades."The SEC's complaint against Beaufort and Kyriacou charges the defendants with violating Section 10(b) of the Securities Exchange Act of 1934 and Rule 10b-5 thereunder.  The SEC also charged Hirschy, Mancino, and their corporate entities with violating Section 17(a)(1) of the Securities Act of 1933, Sections 9(a)(1), 9(a)(2), and 10(b) of the Exchange Act and Rules 10b-5(a) and (c) thereunder.  The SEC is seeking injunctions, disgorgement, prejudgment interest, penalties, and penny stock bars from Beaufort and Kyriacou.  With respect to Hirschy, Mancino, and their corporate entities, the SEC is seeking injunctions, disgorgement, prejudgment interest, penalties, penny stock bars, and an officer-and-director bar against Mancino.The investigation was conducted in the SEC's New York Regional Office by Tejal Shah and Joseph Darragh, Lorraine Collazo, and Michael D. Paley of the Microcap Fraud Task Force and supervised by Lara S. Mehraban, and in Washington, D.C. by Patrick L. Feeney, Robert Nesbitt, and Kevin Guerrero, and supervised by Antonia Chion.  Preethi Krishnamurthy and Ms. Shah will lead the SEC's litigation against Beaufort and Kyriacou.  Ann H. Petalas and Mr. Feeney, under the supervision of Cheryl Crumpton, will handle the SEC's litigation against Mancino, Hirschy, and their entities.  The SEC appreciates the assistance of the Office of the United States Attorney for the Eastern District of New York, the Federal Bureau of Investigation, the Internal Revenue Service, the Alberta Securities Commission, the Ontario Securities Commission, the Financial Conduct Authority of the United Kingdom, and the Financial Industry Regulatory Authority.The Commission's investigation in this matter is continuing."""

input_tokenized = tokenizer.encode(text, return_tensors='pt',padding=padding,pad_to_max_length=True, max_length=6144,truncation=True)
summary_ids = model.generate(input_tokenized,
                                  num_beams=4,
                                  no_repeat_ngram_size=3,
                                  length_penalty=2,
                                  min_length=350,
                                  max_length=500)
summary = [tokenizer.decode(g, skip_special_tokens=True, clean_up_tokenization_spaces=False) for g in summary_ids][0]
### Summary Output
# On March 2, 2018, the Securities and Exchange Commission charged Beaufort Securities Ltd. and Peter Kyriacou, an investment manager at Beaufort, with manipulating the market for HD View 360 Inc., a U.S.-based microcap issuer.  The SEC also announced charges against HD View's CEO, another individual, and three entities they control for manipulating HD View through pump-and-dump schemes.  According to the SEC's complaint, the defendants discussed depositing large blocks of microcap stock in Beaufort accounts, driving up the price of the stock through promotions, manipulating the stock's price and volume through matched trades, and then selling the shares for a large profit.  In a parallel action, the United States Attorney's Office for the Eastern District of New York announced criminal charges against the defendants.  On March 4, the SEC announced the entry of an order suspending trading in the securities of HD View and for West Coast, pending the outcome of a parallel criminal action by the Federal Bureau of Investigation.  Following the announcement of the suspension, HD View stock prices and volume increased significantly, and the defendants agreed to pay over $1.5 million in disgorgement, prejudgment interest, penalties, and an officer and director bar.  Beaufort agreed to settle the charges without admitting or denying the allegations of the complaint, and to pay a $1 million civil penalty. The SEC's investigation, which is continuing, has been conducted by Patrick McCluskey and Cheryl Crumpton of the SEC Enforcement Division's Market Abuse Unit in the New York Regional Office.Â  The SEC appreciates the assistance of the Financial Industry Regulatory Authority of the United Kingdom, the Canadian Securities Commission, the Alberta Securities Commission and the Ontario Securities Commission.
```

## Evaluation results

When the model is used for summarizing legal documents, it achieves the following results:

| Model |  rouge1  |  rouge1-precision  |  rouge2   |  rouge2-precision  | rougeL   |  rougeL-precision  |
|:-----------:|:-----:|:-----:|:------:|:-----:|:------:|:-----:|
| legal-led-base-16384         | **55.69** |  **61.73** | **29.03**  | **36.68** | **32.65**  | **40.43** | 
| led-base-16384          | 29.19  |  30.43  | 15.23  | 16.27 | 16.32  | 16.58 |



