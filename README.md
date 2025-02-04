# CortexPlayground-AntropicClaude3-5
```sql
create or replace database transcript_analysis;

use database transcript_analysis;
use schema public;

create or replace stage docs ENCRYPTION = (TYPE = 'SNOWFLAKE_SSE') DIRECTORY = ( ENABLE = true );

-- Upload earnings call pdf files to internal stage

ls @docs;

create OR replace table transcript_table (call_text TEXT, category VARCHAR(16777216) );

INSERT INTO transcript_table (call_text, category)
SELECT to_varchar(
SNOWFLAKE.CORTEX.PARSE_DOCUMENT(
@docs,
'CORRECTED-TRANSCRIPT_-Snowflake-Inc-SNOW-US-Q3-2025-Earnings-Call-20-November-2024-5_00-PM-ET.pdf',{'mode' : 'LAYOUT'}
)), 'FY25-Q3'
;

INSERT INTO transcript_table (call_text, category)
SELECT to_varchar(
SNOWFLAKE.CORTEX.PARSE_DOCUMENT(
@docs,
'Snowflake-Q4-FY24-Earnings-Call-Transcript.pdf',{'mode' : 'LAYOUT'}
)), 'FY24-Q4'
;

select * from transcript_table;

```
![image](https://github.com/user-attachments/assets/634cab37-878d-4c3c-87ef-6c1d3bf0e5c9)


![image](https://github.com/user-attachments/assets/4189f93e-c942-41e6-8c91-0b445fc3c015)


![image](https://github.com/user-attachments/assets/4d67f1db-09f7-480f-8935-ac84a0763a94)

![image](https://github.com/user-attachments/assets/5d34f489-b101-4f39-94f9-3029f5d6c0c2)

![image](https://github.com/user-attachments/assets/f5686cdc-1156-4c92-a1db-e71afee2a7d6)

![image](https://github.com/user-attachments/assets/c6ea5a6a-8b96-4ba7-b77a-14e212d0540e)

![image](https://github.com/user-attachments/assets/0146a824-cab6-4f26-b22c-299cdec8c63c)

![image](https://github.com/user-attachments/assets/fbcace80-0142-4802-8a01-d57a8ed57ef7)

![image](https://github.com/user-attachments/assets/a03bc355-20da-4aaf-a209-bbbbcbdcda6b)

![image](https://github.com/user-attachments/assets/51db71f8-7759-4b5f-bfa6-fd824bcac575)


```sql
-- You can also run the following in a SQL worksheet and call complete to run Anthropic Claude.
SELECT
  SNOWFLAKE.CORTEX.COMPLETE(
    'claude-3-5-sonnet',
    [{ 'role': 'system', 'content': 'Be careful of halucination. Only answer if you are sure. Can you give me a sentiment analysis of this earnings call?' }, { 'role': 'user', 'content': CALL_TEXT }],
    { }
  ) as TEXT_COMPLETION,
  CALL_TEXT as TEXT_INPUT
FROM
  "TRANSCRIPT_ANALYSIS"."PUBLIC"."TRANSCRIPT_TABLE";
```
