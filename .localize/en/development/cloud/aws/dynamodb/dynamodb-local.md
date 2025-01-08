---
title: DynamoDB-Local
tags:
  - AWS
  - DynamoDB
  - DynamoDB-Local
description:
---

Quote by ChatGPT (as of 10/14/2023).

> DynamoDB Local is a downloadable DynamoDB implementation provided by AWS that allows you to build DynamoDB in a local environment [^1][^2][^3].
> Available as an Apache Maven dependency or Docker image.
>
> Key features and usage of DynamoDB Local.
>
> - **Use in a local environment**: DynamoDB Local allows you to run DynamoDB in a local environment for development and testing.
> - **Docker image**: You can build DynamoDB locally using the "dynamodb-local" Docker image provided by AWS to create tables, items, etc.
> - **Data persistence**: If you want to persist data, you can use the `-v` option to mount the `/data` directory to your local `. /data` directory to mount and store the data.
> - **dynamodb-admin**: There is a tool called "dynamodb-admin", which allows you to manipulate locally built DynamoDB via GUI.

[^1]: [AWS] DynamoDB Local - Qiita. <https://qiita.com/to-fmak/items/3a6df367196ed216b1a4>.
[^2]: DynamoDB local this and that - freee Developers Hub. <https://developers.freee.co.jp/entry/dynamodb-local>.
[^3]: I tried connecting to DynamoDB local with Lambda (Python) | ramble .... <https://ramble.impl.co.jp/3718/>.

!!! note

    The following information is required to use dynamodb-local.

    - Configuration Infomation
    - Credential Infomation

    [Environment variables(export)](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html) Or, you can set it up through [AWS CLI](../aws-cli/index.md).

!!! tips

    To run the program, install [boto3 (AWS SDK for Python)](https://aws.amazon.com/jp/sdk-for-python/).

## Create Table

??? info "create_table.py"

    ```python title="" linenums="1"
    import boto3

    def create_table(dynamodb=None):
        if not dynamodb:
            #dynamodb = boto3.resource('dynamodb',
            #                          region_name='us-west-2',
            #                          endpoint_url="http://localhost:8000",
            #                          aws_access_key_id='localhogehogeid', #arbitrary id for local version
            #                          aws_secret_access_key='localhogehogepw') #arbitrary pw for local version

            dynamodb = boto3.resource('dynamodb', endpoint_url="http://localhost:8000")

        table = dynamodb.create_table(
            TableName='TestTable1',
            KeySchema=[
                {
                    'AttributeName': 'id',
                    'KeyType': 'HASH'  # Partition key
                },
                {
                    'AttributeName': 'date_val',
                    'KeyType': 'RANGE'  # Sort key
                }
            ],
            AttributeDefinitions=[
                {
                    'AttributeName': 'id',
                    'AttributeType': 'S'
                },
                {
                    'AttributeName': 'date_val',
                    'AttributeType': 'S'
                },

            ],
            ProvisionedThroughput={
                'ReadCapacityUnits': 10,
                'WriteCapacityUnits': 10
            }
        )
        return table

    if __name__ == '__main__':
        table = create_table()
        print("Table status:", table.table_status)
    ```

## Get Table Litts

??? info "get_table_lists.py"

    ```python title="" linenums="1"
    import boto3
    import os

    def list_tables(dynamodb=None):
        if not dynamodb:
            #dynamodb = boto3.resource('dynamodb',
            #                          region_name='us-west-2',
            #                          endpoint_url="http://localhost:8000",
            #                          aws_access_key_id='localhogehogeid', #arbitrary id for local version
            #                          aws_secret_access_key='localhogehogepw') #arbitrary pw for local version

            dynamodb = boto3.resource('dynamodb', endpoint_url="http://localhost:8000")

        response = dynamodb.list_tables()
        return response['TableNames']

    if __name__ == '__main__':
        tables = list_tables()
        for table in tables:
            print(table)
    ```

## Add data in bulk

Batch add the following csv files.

??? info "data.csv"

    ```csv title="" linenums="1"
    2023-03-03T00:05:14.071359,ALONE,414.0,1.0,87.99,221.0,135.0,92.04,224.0,125.0,90.19,214.0,130.0,90.72,234.0,130.0,89.26,208.0,140.0,77.73,262.0,160.0,86.77,205.0,185.0,85.84,281.0,210.0,88.13,208.0,240.0,89.21,288.0,260.0,89.16,214.0,280.0,70.95,269.0,270.0,73.1,240.0,285.0,76.71,288.0,340.0,79.44,259.0,345.0,78.61,307.0,395.0,83.3,281.0,395.0
    2023-03-04T00:05:14.487122,ALONE,415.0,1.0,87.26,221.0,135.0,90.62,224.0,125.0,89.26,214.0,130.0,88.96,234.0,130.0,87.7,208.0,140.0,77.78,262.0,160.0,86.47,205.0,185.0,85.11,281.0,210.0,85.35,208.0,240.0,87.94,285.0,260.0,87.84,214.0,280.0,72.22,269.0,270.0,72.02,237.0,285.0,76.71,288.0,340.0,81.84,259.0,345.0,79.39,304.0,395.0,83.35,281.0,395.0
    2023-03-05T00:05:14.890360,ALONE,416.0,1.0,85.99,221.0,136.0,90.48,224.0,126.0,87.3,214.0,131.0,88.62,234.0,131.0,90.82,205.0,141.0,76.61,262.0,161.0,84.18,202.0,186.0,85.99,281.0,211.0,88.82,208.0,241.0,89.21,288.0,261.0,89.5,214.0,281.0,71.53,269.0,271.0,73.19,240.0,286.0,77.05,288.0,336.0,82.23,259.0,346.0,79.83,304.0,396.0,81.1,281.0,391.0
    2023-03-06T00:05:14.524698,ALONE,417.0,1.0,86.62,223.0,137.0,88.82,226.0,126.0,86.96,216.0,132.0,91.06,238.0,132.0,86.13,213.0,143.0,75.15,264.0,161.0,83.89,207.0,184.0,86.18,283.0,208.0,78.08,200.0,225.0,89.16,289.0,260.0,87.55,197.0,248.0,72.71,270.0,272.0,74.07,238.0,284.0,77.59,283.0,342.0,83.84,261.0,348.0,84.33,296.0,406.0,82.32,280.0,395.0
    2023-03-07T00:05:14.930110,ALONE,418.0,1.0,88.04,230.0,134.0,90.72,237.0,124.0,86.87,227.0,129.0,89.65,249.0,129.0,90.19,221.0,139.0,82.71,278.0,163.0,84.52,214.0,178.0,83.15,297.0,216.0,82.96,202.0,221.0,88.09,300.0,264.0,89.5,211.0,211.0,69.19,281.0,279.0,71.63,246.0,289.0,83.5,288.0,342.0,76.71,262.0,347.0,79.93,294.0,400.0,76.81,281.0,395.0
    2023-03-08T00:05:14.358364,ALONE,419.0,1.0,88.62,241.0,131.0,87.94,248.0,126.0,87.99,238.0,131.0,85.84,255.0,126.0,82.57,231.0,136.0,78.37,285.0,161.0,84.86,224.0,171.0,85.55,302.0,216.0,86.57,197.0,201.0,89.31,306.0,266.0,84.38,207.0,156.0,73.68,282.0,276.0,73.68,244.0,281.0,81.98,289.0,346.0,85.84,255.0,351.0,81.1,295.0,406.0,81.35,272.0,401.0
    2023-03-09T00:05:14.762575,ALONE,420.0,1.0,85.45,245.0,138.0,85.11,248.0,127.0,82.08,238.0,132.0,87.3,258.0,127.0,83.84,231.0,138.0,78.27,285.0,163.0,83.5,225.0,168.0,84.67,302.0,220.0,80.81,201.0,204.0,88.28,302.0,266.0,83.45,204.0,168.0,73.58,282.0,276.0,71.63,245.0,286.0,81.59,289.0,343.0,84.28,255.0,348.0,81.4,295.0,409.0,81.59,272.0,399.0
    2023-03-10T00:05:14.193706,ALONE,421.0,1.0,85.84,237.0,135.0,86.82,243.0,124.0,86.91,231.0,129.0,92.77,253.0,129.0,88.18,225.0,140.0,74.32,277.0,167.0,82.76,216.0,177.0,83.64,296.0,215.0,81.84,213.0,231.0,89.6,299.0,263.0,86.77,216.0,268.0,68.36,274.0,274.0,72.71,240.0,284.0,82.96,287.0,343.0,85.74,256.0,354.0,80.81,296.0,402.0,84.28,271.0,402.0
    2023-03-11T00:05:14.594412,ALONE,422.0,1.0,83.54,228.0,129.0,88.67,234.0,124.0,81.88,222.0,124.0,87.94,246.0,124.0,85.99,216.0,135.0,75.88,271.0,166.0,83.69,210.0,181.0,82.62,286.0,218.0,86.28,207.0,233.0,89.01,289.0,265.0,86.18,213.0,275.0,68.9,271.0,275.0,71.09,237.0,291.0,79.83,283.0,343.0,83.59,255.0,353.0,81.25,295.0,405.0,81.69,274.0,400.0
    2023-03-12T00:05:14.234126,ALONE,423.0,1.0,87.74,227.0,131.0,92.63,230.0,121.0,89.84,221.0,126.0,87.35,243.0,126.0,88.53,214.0,137.0,79.54,269.0,163.0,85.35,208.0,179.0,84.96,285.0,216.0,86.43,208.0,232.0,90.23,291.0,263.0,87.21,211.0,274.0,69.38,269.0,279.0,72.31,237.0,285.0,83.15,282.0,343.0,82.23,256.0,353.0,82.62,295.0,401.0,83.79,272.0,401.0
    2023-03-13T00:05:14.634114,ALONE,424.0,1.0,86.18,225.0,131.0,91.55,228.0,121.0,85.11,218.0,126.0,88.57,240.0,126.0,83.25,212.0,137.0,76.76,266.0,163.0,83.15,205.0,184.0,85.16,285.0,216.0,85.3,209.0,237.0,89.99,288.0,263.0,86.28,215.0,274.0,69.43,269.0,279.0,70.12,234.0,290.0,82.23,282.0,343.0,82.86,256.0,353.0,80.27,295.0,401.0,83.64,272.0,401.0
    2023-03-14T00:05:14.066018,ALONE,425.0,1.0,88.48,224.0,130.0,90.38,227.0,119.0,91.7,218.0,130.0,89.11,240.0,130.0,91.6,208.0,140.0,76.56,266.0,161.0,85.64,205.0,183.0,85.21,285.0,215.0,87.6,205.0,236.0,90.97,288.0,262.0,87.65,211.0,278.0,71.39,269.0,273.0,70.46,234.0,289.0,84.18,282.0,342.0,80.27,256.0,353.0,81.05,295.0,400.0,84.38,272.0,400.0
    2023-03-15T00:05:14.466080,ALONE,426.0,1.0,86.08,221.0,135.0,90.48,227.0,125.0,87.65,218.0,130.0,92.04,237.0,130.0,89.6,208.0,141.0,76.37,265.0,162.0,85.45,205.0,183.0,84.81,284.0,215.0,89.16,205.0,236.0,90.04,287.0,262.0,89.75,212.0,278.0,70.21,268.0,278.0,71.34,234.0,289.0,82.23,281.0,342.0,81.88,256.0,353.0,78.56,293.0,405.0,83.64,271.0,400.0
    2023-03-16T00:05:14.898491,ALONE,427.0,1.0,90.28,218.0,135.0,88.77,221.0,124.0,81.79,212.0,130.0,90.19,234.0,130.0,86.23,208.0,145.0,77.15,262.0,161.0,85.6,202.0,183.0,85.3,281.0,215.0,88.09,205.0,236.0,91.41,287.0,262.0,89.75,212.0,278.0,71.24,268.0,273.0,71.34,234.0,289.0,81.79,281.0,342.0,80.37,256.0,353.0,79.1,293.0,400.0,83.25,271.0,400.0
    2023-03-17T00:05:14.298281,ALONE,428.0,1.0,89.11,223.0,139.0,90.43,227.0,129.0,88.72,220.0,134.0,92.87,241.0,129.0,87.16,213.0,144.0,76.12,268.0,160.0,87.65,213.0,187.0,84.42,293.0,218.0,88.09,217.0,239.0,91.31,296.0,266.0,87.7,220.0,276.0,74.12,279.0,276.0,71.92,244.0,287.0,79.05,303.0,345.0,78.17,258.0,350.0,82.62,331.0,402.0,87.3,275.0,402.0
    2023-03-18T00:05:14.938566,ALONE,429.0,1.0,82.57,251.0,140.0,81.35,254.0,134.0,79.59,245.0,140.0,87.21,269.0,128.0,74.56,242.0,147.0,75.1,297.0,165.0,84.57,251.0,184.0,71.68,318.0,209.0,84.23,251.0,228.0,89.11,330.0,271.0,90.28,254.0,271.0,68.7,309.0,271.0,74.41,281.0,271.0,83.79,321.0,340.0,78.52,278.0,334.0,79.69,336.0,402.0,85.69,288.0,396.0
    2023-03-19T00:05:14.338086,ALONE,430.0,1.0,84.52,253.0,129.0,89.21,256.0,124.0,74.32,247.0,129.0,91.6,274.0,129.0,38.45,271.0,129.0,76.37,301.0,162.0,80.03,262.0,179.0,80.13,322.0,218.0,83.11,268.0,229.0,87.6,319.0,273.0,86.62,271.0,267.0,55.62,319.0,267.0,66.5,295.0,278.0,72.41,322.0,345.0,79.15,310.0,345.0,77.15,334.0,406.0,71.39,328.0,395.0
    2023-03-20T00:05:14.767194,ALONE,431.0,1.0,84.38,257.0,158.0,89.75,260.0,147.0,86.04,251.0,152.0,88.13,274.0,147.0,70.07,248.0,158.0,76.32,294.0,179.0,82.81,266.0,194.0,77.05,323.0,226.0,86.62,274.0,242.0,91.65,323.0,284.0,87.06,277.0,278.0,63.67,335.0,268.0,59.67,306.0,278.0,88.33,317.0,347.0,86.77,297.0,336.0,80.32,352.0,394.0,82.37,329.0,394.0
    2023-03-21T00:05:14.170348,ALONE,432.0,1.0,88.96,264.0,212.0,93.16,267.0,200.0,93.26,257.0,208.0,92.58,284.0,200.0,83.69,254.0,212.0,74.51,308.0,232.0,69.38,277.0,236.0,80.18,332.0,287.0,68.16,288.0,287.0,88.28,332.0,338.0,74.61,288.0,323.0,61.23,332.0,326.0,63.57,305.0,319.0,85.45,339.0,397.0,88.53,281.0,366.0,63.13,390.0,371.0,77.78,328.0,397.0
    2023-03-22T00:05:14.598332,ALONE,433.0,1.0,87.21,273.0,235.0,92.58,280.0,227.0,88.67,270.0,235.0,93.07,297.0,227.0,86.62,263.0,239.0,74.61,321.0,263.0,73.29,287.0,263.0,77.54,345.0,316.0,63.87,297.0,316.0,90.09,345.0,364.0,52.54,297.0,340.0,54.88,345.0,344.0,60.25,317.0,336.0,85.01,348.0,417.0,91.11,280.0,364.0,75.78,392.0,377.0,79.54,331.0,397.0
    2023-03-23T00:05:14.002020,ALONE,434.0,1.0,91.36,285.0,240.0,93.7,292.0,228.0,90.58,278.0,232.0,91.7,310.0,232.0,75.1,278.0,240.0,76.27,338.0,268.0,72.22,303.0,264.0,80.76,363.0,325.0,40.62,310.0,305.0,88.77,363.0,373.0,43.7,306.0,333.0,62.6,349.0,345.0,61.72,324.0,337.0,85.79,349.0,418.0,88.67,285.0,365.0,58.69,398.0,382.0,78.56,334.0,386.0
    2023-03-24T00:05:14.424593,ALONE,435.0,1.0,89.5,283.0,232.0,91.36,290.0,224.0,90.33,280.0,228.0,94.34,310.0,232.0,69.82,280.0,232.0,75.73,337.0,269.0,75.49,303.0,265.0,72.46,358.0,325.0,50.73,307.0,305.0,87.26,358.0,373.0,55.86,307.0,333.0,60.84,351.0,349.0,63.23,324.0,337.0,83.74,348.0,417.0,87.7,286.0,365.0,53.47,395.0,377.0,77.2,337.0,385.0
    2023-03-25T00:05:14.042123,ALONE,436.0,1.0,89.79,284.0,233.0,95.41,291.0,225.0,91.8,280.0,229.0,97.46,307.0,229.0,85.69,272.0,233.0,78.96,331.0,266.0,73.49,299.0,262.0,82.23,354.0,322.0,59.33,307.0,302.0,87.74,354.0,374.0,55.57,303.0,334.0,63.57,354.0,346.0,57.81,323.0,334.0,82.08,347.0,418.0,88.43,288.0,366.0,58.25,398.0,378.0,79.0,339.0,386.0
    2023-03-26T00:05:14.474056,ALONE,437.0,1.0,86.28,278.0,238.0,90.77,285.0,229.0,88.53,271.0,233.0,92.72,303.0,233.0,82.47,268.0,238.0,76.03,327.0,270.0,70.46,292.0,266.0,77.64,348.0,322.0,47.63,306.0,306.0,85.89,351.0,374.0,43.99,306.0,338.0,51.27,358.0,346.0,60.01,320.0,338.0,85.11,344.0,418.0,86.18,285.0,362.0,60.16,396.0,378.0,81.25,337.0,390.0
    2023-03-27T00:05:14.874079,ALONE,438.0,1.0,87.26,271.0,242.0,92.87,278.0,234.0,91.36,267.0,238.0,93.99,296.0,234.0,79.88,264.0,242.0,77.93,322.0,270.0,65.58,296.0,262.0,76.07,348.0,323.0,35.74,318.0,307.0,88.09,348.0,376.0,26.98,311.0,332.0,56.3,358.0,348.0,53.47,326.0,340.0,84.47,344.0,421.0,88.04,286.0,364.0,73.58,395.0,381.0,79.83,337.0,393.0
    2023-03-28T00:05:14.302648,ALONE,439.0,1.0,86.62,270.0,252.0,89.45,274.0,244.0,90.33,266.0,248.0,89.01,292.0,240.0,52.49,296.0,236.0,73.44,314.0,284.0,68.55,318.0,248.0,71.73,333.0,339.0,18.09,314.0,303.0,90.38,333.0,386.0,19.02,307.0,307.0,56.64,358.0,354.0,41.99,362.0,335.0,86.33,347.0,421.0,80.96,296.0,343.0,40.11,400.0,407.0,36.5,333.0,398.0
    2023-03-29T00:05:14.706294,ALONE,440.0,1.0,83.2,282.0,242.0,89.94,285.0,234.0,76.81,279.0,234.0,88.04,304.0,227.0,61.04,310.0,223.0,74.85,320.0,271.0,77.78,338.0,246.0,77.54,326.0,331.0,34.23,351.0,290.0,82.52,320.0,379.0,17.36,357.0,320.0,53.03,351.0,353.0,41.38,363.0,342.0,81.98,348.0,420.0,46.22,357.0,394.0,68.31,410.0,405.0,81.3,410.0,386.0
    2023-03-30T00:05:14.134645,ALONE,441.0,1.0,90.38,274.0,254.0,91.02,278.0,247.0,85.4,270.0,247.0,96.39,299.0,239.0,56.84,299.0,239.0,81.49,319.0,273.0,60.21,315.0,265.0,80.03,340.0,325.0,66.16,336.0,321.0,84.52,340.0,381.0,82.67,332.0,355.0,63.77,369.0,355.0,42.31,365.0,351.0,85.89,352.0,418.0,57.08,365.0,403.0,59.28,414.0,399.0,58.54,414.0,388.0
    ```

??? info "bulk_incert.py"

    ```python title="" linenums="1"
    import pandas as pd
    import boto3
    from decimal import Decimal

    def bulk_insert(file_name, table_name, dynamodb=None):
        if not dynamodb:
            #dynamodb = boto3.resource('dynamodb',
            #                          region_name='us-west-2',
            #                          endpoint_url="http://localhost:8000",
            #                          aws_access_key_id='localhogehogeid', #arbitrary id for local version
            #                          aws_secret_access_key='localhogehogepw') #arbitrary pw for local version

            dynamodb = boto3.resource('dynamodb', endpoint_url="http://localhost:8000")

        table = dynamodb.Table(table_name)
        df = pd.read_csv(file_name, header=None)  # Read data as is
        with table.batch_writer() as batch:
            for index, row in df.iterrows():
                item = {
                    'id': str(index),  # Use the row index as the id
                    'date_val': str(row[0]),  # Access the date column by its index
                }
                # Add other columns here
                for i in range(1, 55):  # Assuming there are 53 data columns
                    try:
                        # Try to convert to Decimal
                        value = float(row[i])
                        print(f"value={value}")
                        if value.is_integer():
                            item[f'data{i}'] = Decimal(f'{value:.1f}')
                            print(f"int: {item[f'data{i}']}")
                        else:
                            item[f'data{i}'] = Decimal(str(row[i]))
                            print(f"str: {item[f'data{i}']}")
                    except:
                        # If it fails, keep it as a string
                        item[f'data{i}'] = str(row[i])
                batch.put_item(Item=item)

    if __name__ == '__main__':
        bulk_insert('data.csv', 'TestTable1')
    ```

## Get Data

??? info "get_items_float.py"

    ```python title="" linenums="1"
    import boto3
    from decimal import Decimal

    def get_items(date_from, date_to, table_name, dynamodb=None):
        if not dynamodb:
            #dynamodb = boto3.resource('dynamodb',
            #                          region_name='us-west-2',
            #                          endpoint_url="http://localhost:8000",
            #                          aws_access_key_id='localhogehogeid', #arbitrary id for local version
            #                          aws_secret_access_key='localhogehogepw') #arbitrary pw for local version

            dynamodb = boto3.resource('dynamodb', endpoint_url="http://localhost:8000")

        table = dynamodb.Table(table_name)
        response = table.scan(
            FilterExpression="date_val between :date_from and :date_to",
            ExpressionAttributeValues={
                ':date_from': date_from,
                ':date_to': date_to
            }
        )
        items = response['Items']
        for item in items:
            for key, value in item.items():
                if isinstance(value, Decimal):
                    if value % 1 == 0:
                        item[key] = f'{value:.1f}'
                    else:
                        item[key] = float(value)
        return items

    if __name__ == '__main__':
        items = get_items('2023-03-03', '2023-03-06', 'TestTable1')
        for item in items:
            print(item)
    ```

## Save Data(DataFrame to CSV)

The code outputs the same file as the original data (data.csv) to a CSV file (get_tbl_data.csv) based on the information obtained from dynamodb-local for operation check.

??? info "get_tbl_data_to_sort_csv.py"

    ```python title="" linenums="1"
    import boto3
    import pandas as pd
    from decimal import Decimal

    def get_items(date_from, date_to, table_name, dynamodb=None):
        if not dynamodb:
            #dynamodb = boto3.resource('dynamodb',
            #                          region_name='us-west-2',
            #                          endpoint_url="http://localhost:8000",
            #                          aws_access_key_id='localhogehogeid', #arbitrary id for local version
            #                          aws_secret_access_key='localhogehogepw') #arbitrary pw for local version

            dynamodb = boto3.resource('dynamodb', endpoint_url="http://localhost:8000")

        table = dynamodb.Table(table_name)
        response = table.scan(
            FilterExpression="date_val between :date_from and :date_to",
            ExpressionAttributeValues={
                ':date_from': date_from,
                ':date_to': date_to
            }
        )
        # If not formatted as 100.0, return below.
        #return response['Items']

        items = response['Items']
        for item in items:
            for key, value in item.items():
                if isinstance(value, Decimal):
                    if value % 1 == 0:
                        item[key] = f'{value:.1f}'
                    else:
                        item[key] = float(value)
        return items

    def save_to_csv(items, file_name):
        df = pd.DataFrame(items)
        # Reorder the columns in the desired order
        df = df[['date_val'] + [f'data{i}' for i in range(1, 55)]]
        # Sort the DataFrame by date_val in ascending order
        df = df.sort_values('date_val')
        df.to_csv(file_name, index=False, header=False)

    if __name__ == '__main__':
        items = get_items('2023-03-03', '2023-03-31', 'TestTable1')
        save_to_csv(items, 'get_tbl_data.csv')
    ```
