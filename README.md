# uts_slamet

# data
company_name_list = [{'name': 'Company 1'},
          {'name': 'Company 2'},
          {'name': 'Company 3'}]

employee_name_list = [{'name': 'John Doe'},
          {'name': 'Tom Smith'},
          {'name': 'Andrew Sebastian'}]

company_detail_list = {
      'Company 1': {
          'name': 'Company 1',
          'domain': 'Retail',
          'clients': [
              {
                  'name': 'acme.inc',
                  'country': 'united states'
              },
              {
                  'name': 'Wayne.co',
                  'country': 'united states'
              }
          ]
      },
      'Company 2': {
          'name': 'Company 2',
          'domain': 'Construction',
          'clients': [
              {
                  'name': 'Tesla',
                  'country': 'united states'
              },
              {
                  'name': 'Japan Airlines',
                  'country': 'japan'
              },
              {
                  'name': 'Indofood',
                  'country': 'indonesia'
              }
          ]
      },
      'Company 3': {
          'name': 'Company 3',
          'domain': 'Healthcare',
          'clients': [
              {
                  'name': 'Petronas',
                  'country': 'malaysia'
              },
              {
                  'name': 'VW Group',
                  'country': 'germany'
              },
              {
                  'name': 'IBM',
                  'country': 'united states'
              },
              {
                  'name': 'Mitsubishi',
                  'country': 'japan'
              }
          ]
      }
  }

employee_detail_list = {
      'John Doe': {
          'name': 'EMP-0001',
          'first_name': 'John',
          'last_name': 'Doe',
          'full_name': 'John Doe',
          'company': 'Company 1'
      },
      'Tom Smith': {
          'name': 'EMP-0002',
          'first_name': 'Tom',
          'last_name': 'Smith',
          'full_name': 'Tom Smith',
          'company': 'Company 2'
      },
      'Andrew Sebastian': {
          'name': 'EMP-0003',
          'first_name': 'Andrew',
          'last_name': 'Sebastian',
          'full_name': 'Andrew Sebastian',
          'company': 'Company 2'
      },
  }


Soal No 1
  def sort_company():
    sorted_companies = sorted(company_detail_list.values(), key=lambda x: x['domain'], reverse=True)
    return [{'name': company['name'], 'domain': company['domain']} for company in sorted_companies]

# Test function
sorted_companies = sort_company()
print(sorted_companies)

[{'name': 'Company 1', 'domain': 'Retail'}, {'name': 'Company 3', 'domain': 'Healthcare'}, {'name': 'Company 2', 'domain': 'Construction'}]


Soal No 2
def print_company_domains():
    for company_name, company_detail in company_detail_list.items():
        num_clients = len(company_detail['clients'])
        print(f"{company_name}: {company_detail['domain']}, relation: {num_clients} clients")

# Test function
print_company_domains()

Company 1: Retail, relation: 2 clients
Company 2: Construction, relation: 3 clients
Company 3: Healthcare, relation: 4 clients


Soal no 3
def get_employee_domains():
    employee_domains = []
    for employee_name, employee_detail in employee_detail_list.items():
        company_name = employee_detail['company']
        domain = company_detail_list[company_name]['domain']
        employee_domains.append({
            "full_name": employee_detail['full_name'],
            "company": company_name,
            "domain": domain
        })
    return employee_domains

# Test function
employee_domains = get_employee_domains()
print(employee_domains)

[{'full_name': 'John Doe', 'company': 'Company 1', 'domain': 'Retail'}, {'full_name': 'Tom Smith', 'company': 'Company 2', 'domain': 'Construction'}, {'full_name': 'Andrew Sebastian', 'company': 'Company 2', 'domain': 'Construction'}]


Soal no 4
def get_company_employees():
    company_employees = []
    for company_name in company_detail_list.keys():
        employees = [employee['full_name'] for employee in employee_detail_list.values() if employee['company'] == company_name]
        company_employees.append({
            "company": company_name,
            "employees": employees
        })
    return company_employees

# Test function
company_employees = get_company_employees()
print(company_employees)

[{'company': 'Company 1', 'employees': ['John Doe']}, {'company': 'Company 2', 'employees': ['Tom Smith', 'Andrew Sebastian']}, {'company': 'Company 3', 'employees': []}]

SOAL PRE PROCESSING DATA

import pandas as pd
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder, StandardScaler

data_startup = pd.DataFrame({
    'R&D Spend': [1000.23, float('nan'), 76253.86, 63408.86, 28754.33, 119943.24, 134615.46],
    'Administration': [124153.04, 110594.11, 113867.30, 129219.61, 118546.05, 156547.42, 147198.87],
    'Marketing Spend': [1903.93, 229160.95, 298664.47, 46085.25, 172295.67, 256512.92, 127716.82],
    'State': ['New York', 'Florida', 'California', 'California', 'California', 'Florida', 'California'],
    'Profit': [64926.08, 146121.95, 118474.03, 97427.84, 78239.91, 132602.65, 156122.51]
})

data_startup['R&D Spend'].fillna(data_startup['R&D Spend'].mean(), inplace=True)

ct = ColumnTransformer(transformers=[('encoder', OneHotEncoder(), ['State'])], remainder='passthrough')
data_startup = pd.DataFrame(ct.fit_transform(data_startup))

data_startup.columns = ['State_New York', 'State_Florida', 'State_California', 'R&D Spend', 'Administration', 'Marketing Spend', 'Profit']

data_startup['Tax'] = (data_startup['Profit'] + data_startup['Marketing Spend'] + data_startup['Administration']) * 0.05

scaler = StandardScaler()
data_startup[['R&D Spend', 'Administration', 'Marketing Spend', 'Profit', 'Tax']] = scaler.fit_transform(data_startup[['R&D Spend', 'Administration', 'Marketing Spend', 'Profit', 'Tax']])

data_startup.index += 1

print(data_startup)

State_New York  State_Florida  State_California  R&D Spend  Administration  \
1             0.0            0.0               1.0  -1.603505       -0.277473   
2             0.0            1.0               0.0   0.000000       -1.125502   
3             1.0            0.0               0.0   0.128699       -0.920784   
4             1.0            0.0               0.0  -0.166970        0.039410   
5             1.0            0.0               0.0  -0.964655       -0.628156   
6             0.0            1.0               0.0   1.134351        1.748600   
7             1.0            0.0               0.0   1.472079        1.163905   

   Marketing Spend    Profit       Tax  
1        -1.571128 -1.519197 -1.717386  
2         0.662403  1.024661  0.662693  
3         1.345499  0.158454  1.026926  
4        -1.136905 -0.500921 -1.057588  
5         0.103519 -1.102077 -0.279959  
6         0.931224  0.601102  1.145224  
7        -0.334612  1.337977  0.220091
