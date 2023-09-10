# nyc-payrol-analytics
Udacity NYC Payroll Analytics

-- Storage path where the result set will persist
IF NOT EXISTS (SELECT * FROM sys.external_data_sources WHERE name = 'adlsnycpayroll-yourfirstname-lastintial_adlsnycpayroll-yourfirstname-lastintial_dfs_core_windows_net') 
    CREATE EXTERNAL DATA SOURCE [nycpayroll_dfs_core_windows_net] 
    WITH (
        LOCATION = 'abfss://adlsnycpayroll-yourfirstname-lastintial@adlsnycpayroll-yourfirstname-lastintial.dfs.core.windows.net' 
    )
GO

-- Use the same file format as used for creating the External Tables during the LOAD step.
IF NOT EXISTS (SELECT * FROM sys.external_file_formats WHERE name = 'SynapseDelimitedTextFormat') 
    CREATE EXTERNAL FILE FORMAT [SynapseDelimitedTextFormat] 
    WITH ( FORMAT_TYPE = DELIMITEDTEXT ,
           FORMAT_OPTIONS (
             FIELD_TERMINATOR = ',',
             USE_TYPE_DEFAULT = FALSE
            ))
GO

CREATE EXTERNAL TABLE [dbo].[NYC_Payroll_EMP_MD](
    [EmployeeID] [varchar](10) NULL,
    [LastName] [varchar](20) NULL,
    [FirstName] [varchar](20) NULL
)
WITH (
	    LOCATION = 'payroll_emp_md.csv',
        DATA_SOURCE = [nycpayroll_dfs_core_windows_net],
        FILE_FORMAT = [SynapseDelimitedTextFormat]
)
GO

CREATE EXTERNAL TABLE [dbo].[NYC_Payroll_TITLE_MD](
    [TitleCode] [varchar](10) NULL,
    [TitleDescription] [varchar](100) NULL
)
WITH (
		LOCATION = 'payroll_title_md.csv',
        DATA_SOURCE = [nycpayroll_dfs_core_windows_net],
        FILE_FORMAT = [SynapseDelimitedTextFormat]
)
GO

CREATE EXTERNAL TABLE [dbo].[NYC_Payroll_AGENCY_MD](
    [AgencyID] [varchar](10) NULL,
    [AgencyName] [varchar](50) NULL
)
WITH (
	    LOCATION = 'payroll_agency_md.csv',
        DATA_SOURCE = [nycpayroll_dfs_core_windows_net],
        FILE_FORMAT = [SynapseDelimitedTextFormat]
)
GO

CREATE EXTERNAL TABLE [dbo].[NYC_Payroll_Data](
    [FiscalYear] [int] NULL,
    [PayrollNumber] [int] NULL,
    [AgencyID] [varchar](10) NULL,
    [AgencyName] [varchar](50) NULL,
    [EmployeeID] [varchar](10) NULL,
    [LastName] [varchar](20) NULL,
    [FirstName] [varchar](20) NULL,
    [AgencyStartDate] [date] NULL,
    [WorkLocationBorough] [varchar](50) NULL,
    [TitleCode] [varchar](10) NULL,
    [TitleDescription] [varchar](100) NULL,
    [LeaveStatusasofJune30] [varchar](50) NULL,
    [BaseSalary] [float] NULL,
    [PayBasis] [varchar](50) NULL,
    [RegularHours] [float] NULL,
    [RegularGrossPaid] [float] NULL,
    [OTHours] [float] NULL,
    [TotalOTPaid] [float] NULL,
    [TotalOtherPay] [float] NULL
) 
WITH (
		LOCATION = 'payroll_data.csv',
        DATA_SOURCE = [nycpayroll_dfs_core_windows_net],
        FILE_FORMAT = [SynapseDelimitedTextFormat]
)
GO