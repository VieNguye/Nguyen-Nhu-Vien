Provide your CLI command here:

jq -r 'select(.symbol == "TSLA" and .side == "sell") | .order_id' ./transaction-log.txt | while read order_id; do
  curl -s "https://example.com/api/$order_id"
done > ./output.txt
