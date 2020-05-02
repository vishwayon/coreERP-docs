Designing Reports
-----------------

All reports in CoreERP are to be designed using Jaspersoft Reports. This is an xml based report designer.

Setup Jaspersoft Designer
~~~~~~~~~~~~~~~~~~~~~~~~~

Follow the following steps carefully for initial Jasperreports setup.

    1. Download TIBCOJaspersoft Studio from //ishasoftstorage/public/coreERP-essentials/jasper-reports. This is a tar.gz file. Uncompress it in a seperate folder on your local machine preferably as /opt/jasper-studio. Also available at the same location is the *Jaspersoft Studio user Guide.pdf*. Read this for a head start with report designer.

    2. Start **Jaspersoft Studio**. It would request you to create a Jaspersoft Projects folder in your home directory. Proceed to do the same.

    3. Click on *File->New Project* and create a new project called CoreReports in the Jaspersoft Projects folder.

    4. Go to a terminal window and move to the newly created CoreReports project folder. Create symbolic links to core and cwf in this folder. If you are working on more modules, create the appropriate links.

    5. Return to Jaspersoft Studio and refresh the project. It would display the core and cwf folders with all project files.

Designing rules for all reports in CoreERP
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

    1. When you want to create a new report, first open cwf/report-templates/basic-template.jrxml. **DO NOT MODIFY THIS TEMPLATE FILE**

    2. Do a *File->Save As* and save the report to the required folder in **core** with a new report name. After saving it with a new name, you can proceed to design the report.

    3. The report already has the following:

        - A Style template. Apply these styles to each element placed on the report
        - Standard **cwf_** parameters that can be used. Apply these for date_format, currency_format, etc.
        - **DO NOT MODIFY PAGE MARGINS.** However, you can set Portrait or landscape.
        - Use the **preport_period** parameter for passing the report period.

    4. Do not modify the Title subreport and/or report caption placement. Only edit the **Report Title** text for the name of the report. This is a static text control. Do not change styles or placement of the object. This should be changed in 2 places. Once in the Title and other in the page header.

    5. Do not modify **Page Footer**.

    6. If you are required to pass a date parameter to the report, declare the parameter as string in the report. While passing it to the Dataset SQL command, use the sql syntax to convert to Date. e.g: $P{pfrom_date} **::Date**.

If all the aforementioned rules are followed while designing reports, it would ensure that all reports comply with acceptable standards of design.

You may refer to the TrialBalance or GeneralLedger for already designed reports.


Jasper Report Export Settings
-----------------------------

Jasper Reports requires the following settings along with the ttf in a jar file for exporting reports in html and pdf properly.

We create a file called report-fonts.jar. This should contain the following:

    - File *jasperreports_extension.properties* with following contents ::
    
        net.sf.jasperreports.extension.registry.factory.fonts=net.sf.jasperreports.engine.fonts.SimpleFontExtensionsRegistryFactory
        net.sf.jasperreports.extension.simple.font.families.1=fonts/fontsfamily.xml

    - Create a sub folder *fonts*. It should contain
        - File *fontsfamily.xml* with following data ::
            
            <?xml version="1.0" encoding="UTF-8"?>
            <fontFamilies>
              <fontFamily name="Liberation Sans">
                <normal>fonts/LiberationSans/LiberationSans-Regular.ttf</normal>
                <bold>fonts/LiberationSans/LiberationSans-Bold.ttf</bold>
                <italic>fonts/LiberationSans/LiberationSans-Italic.ttf</italic>
                <boldItalic>fonts/LiberationSans/LiberationSans-BoldItalic.ttf</boldItalic>
                <pdfEmbedded>true</pdfEmbedded>
                <exportFonts>
                </exportFonts>
                <locales>
                  <locale>en</locale>
                </locales>
              </fontFamily>
            </fontFamilies>

        - Create sub folder for each fontFamily inside folder *fonts*
        - Copy the ttf files of each font that is listed in the element value. e.g. normal requires LiberationSans-Regular.ttf

    - Build the jar file and put it into /opt/tomcat/jaspersoft/

This jar would be automatically loaded and the fonts mentioned would be embedded and exported in pdf.


