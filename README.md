# Assignment for Data Analyst Role 
## Solution
### Task - 1
*Please provide steps to convert an epoch Timestamp to  
a. Datetime value  
b. Date value  
with any visualisation/wrangling tool (preferred Power Query) except google
sheets and microsoft excel.*

### Task - 2
### Task - 3
*Please use python to wrangle the following*
| Date       | Devices                 | Issues                                  | Organisation | Pending |
|------------|--------------------------|-----------------------------------------|--------------|---------|
| 2024-01-01 | [abcdef, xyzzyx, ababcd] | [emergency_stop, manual, remote_stop]   | EV_one       | Yes     |
| 2024-01-02 | [opqrst, rd77dr]         | [remote_stop, emergency]                | EV_two       | No      |
| 2024-01-12 | xvynmare                 | remote_stop                             | EV_three     | Yes     |


**Code**
````
import pandas as pd
from io import StringIO

# Given data
data = """Date,Devices,Issues,Organisation,Pending
2024-01-01,"[abcdef, xyzzyx, ababcd]","[emergency_stop, manual, remote_stop]",EV_one,Yes
2024-01-02,"[opqrst, rd77dr]","[remote_stop, emergency]",EV_two,No
2024-01-12,xvynmare,remote_stop,EV_three,Yes
"""

df = pd.read_csv(StringIO(data))

# Function to properly explode the columns
def wrangle_columns(df):
    # Split devices and issues into lists
    df['Devices'] = df['Devices'].str.strip('[]').str.split(', ')
    df['Issues'] = df['Issues'].str.strip('[]').str.split(', ')
    
    # Ensure devices and issues are lists
    df['Devices'] = df['Devices'].apply(lambda x: [x] if not isinstance(x, list) else x)
    df['Issues'] = df['Issues'].apply(lambda x: [x] if not isinstance(x, list) else x)
    
    # Exploding the rows properly by creating a new DataFrame for wrangle rows
    wrangle_rows = []
    for idx, row in df.iterrows():
        devices = row['Devices']
        issues = row['Issues']
        organisation = row['Organisation']
        pending = row['Pending']
        date = row['Date']
        
        # Pair each device with the corresponding issue
        for device, issue in zip(devices, issues):
            wrangle_rows.append([date, device.strip(), issue.strip(), organisation, pending])
    
    # Create a new DataFrame from the exploded rows
    return pd.DataFrame(exploded_rows, columns=['Date', 'Devices', 'Issues', 'Organisation', 'Pending'])

# Apply the explode function
df = wrangle_columns(df)

# Displaying the final result
print(df)
````

****Source-link****

[assignment_data_analyst](https://colab.research.google.com/drive/1tTzC2RzYfaUnPMRLKk4tKF4_Sqty395Y?usp=sharing)


**Output**
<p align="">
  <img src="https://github.com/fileediting/kazam_data_analyst_assignment/blob/main/img/Task-3-output.png" alt="Task-3-output">
</p>

### Task - 4
