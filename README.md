# File-Cleaning-Python-Script
This file cleans data from various EHRs.



import traceback
filter_list = []
output = open('CleanedFile.csv', 'w')
output.write('MRN, Last Name, First Name, Sex, DOB, Race/Ethnicity, Email, Home, Work, Mobile\n')
line = ""
dict = {}
try:
	with open('Berlioz_Age8.txt', 'r') as NM:
		skip = True;
		for line in NM:
			if "Patient #" in line:
				skip = False
				continue
			if skip == True:
				continue
			if "Clinical Analysis Report by Patient" in line :
				continue
			if "Medical Colleagues of Texas" in line:
				continue
			if "Total Patients" in line:
				continue
			if len(line.strip()) == 0:
				continue
		#	print ( line.strip())
			fields = line.split("\t")
			dict[fields[0].strip()] = "Y"
			#print( fields[0] + ", "+ fields[1] + ", " + fields[2] + ", " + fields[3])
	with open('MCT_All.txt', 'r') as f:
	    for line in f:
	    	if 'Default Account:' in line:
	            print(line)
	            firstname = ''
	            lastname = ''
	            line = line.split('Default Account:')[0]
	            mrn = line.split('\t')[0].strip()
	            name = line.split('\t')[1].strip()
	            try:
	                firstname = name.split(',')[1]
	            except:
	                print ('no name')

	            lastname = name.split(',')[0]

	            line = next(f)

	            line = line.split('Sex:')[1].strip()
	            sex = line.split('Emp Status:')[0].strip()

	            line = next(f)

	            line = line.split('DOB:')[1].strip()
	            dob = line.split('Employee ID:')[0].strip()

	            line = next(f)

	            line = line.split('Race / Ethnicity:')[1].strip()
	            race = line.split('Employer:')[0].strip()
	            
	            line = next(f)
	            line = next(f)
	            

	            while True:
	                line = next(f)
	                if 'Email:' in line:
	                    email = line.split('Email:')[1].strip()
	                    break
	            while True:
	                line = next(f)
	                if 'Home:' in line:
	                    line = line.split('Home:')[1].strip()
	                    home = line.split('Consent:')[0].strip()
	                    break
 
	            while True:
	                line = next(f)
	                if 'Work:' in line:
	                    line = line.split('Work:')[1].strip()
	                    work = line.split('Referral Src:')[0].strip()
	                    break
         
	            while True:
	                line = next(f)
	                if 'Mobile' in line:
	                    mobile = line.split(':')[1].strip()
	                    break
	            vaccinated = "N"
	            if mrn.strip() in dict:
	            	vaccinated = "Y"


	            output.write('"' + mrn + '","' + lastname + '","'
	                         + firstname + '","' + sex + '","' + dob + '","' + race + '","' + email
	                         + '","' + home + '","' + work + '","' + mobile + '","' + vaccinated
	                         + '"\n')

except Exception as e:
    print(line)
    print(traceback.format_exc())


output.close()

