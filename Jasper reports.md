# Jasper Reports

## Build from source

[Jasper Wiki](http://community.jaspersoft.com/wiki/contributing-jaspersoft-studio-and-building-sources)

Used Juno 3.8.2 Exclipse release

Create workspace

Change compiler preferences to Java 1.6

Install subclipse "Help" "Install software" Work with: http://subclipse.tigris.org/update_1.10.x

Load team working set 

* jaspersoftStudioCE-plugin.psf -> eclipse perspective 
* jaspersoftStudioCE-rcp.psf -> stand alone application (missing dependencies?)

with eclipse import.

Preferences > Plug-in Development > Target Platform section.
Import JSS382 (extracted) (as Binary)

Add from ...repos/jasperstudio:

* ...i18n.feature
* ...jre.feature
* ....rcp
* ...rcp.feature
* ...rcp.product

Launch the jasperstudio.product as debug/application -> will fail

Modify the launch configuraton, plugin tab, Add required plugins -> some functions are mssing so add all ...

-----


Add [SWT](http://www.eclipse.org/swt/updates/3.8)

Add [GEF](http://download.eclipse.org/tools/gef/updates/releases/)

Add [Nebula](http://download.eclipse.org/technology/nebula/snapshot)

## Modify library

[basics about lib](http://community.jaspersoft.com/wiki/jasperreports-library-tutorial)

To modify <code>net.sf.jasperreports.engine.util.SimpleFileResolver</code>

Use [for Jasper report library project](http://sourceforge.net/projects/jasperreports/files/jasperreports/JasperReports%206.1.0/) Have to fix some 6.1.0 to 6.1.1 version incompatiblities.

Use svn [reports lib svn trunk version 6](http://code.jaspersoft.com/svn/repos/jasperreports/)

extracted with extra upper dir as workspace

Modify:

```
net.sf.jasperreports.engine.base.VirtualizableElementList
```

```
// Need to override as eclipse 3.8.2 doesnt unterstand both base implementations
public Spliterator<JRPrintElement> spliterator() {
    return Spliterators.spliterator(this, Spliterator.ORDERED);
}
```

Add additional packages to <code>manifest.mf</code> export packages;

Replace SimpleFileResolver build <code>jar</code> with <code>ant clean; ant jar</code>

## Todo

* Use DB-Based reports
* translation various messages.properties
* strip unnessary sources?
* remove community dependency (calling home functions)
* Simplify bean usage

## Java library usage

<code>
```
<?xml version="1.0" encoding="UTF-8"?>
<jasperReport xmlns="http://jasperreports.sourceforge.net/jasperreports" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://jasperreports.sourceforge.net/jasperreports http://jasperreports.sourceforge.net/xsd/jasperreport.xsd" name="report" pageWidth="595" pageHeight="842" columnWidth="555" leftMargin="20" rightMargin="20" topMargin="30" bottomMargin="30" uuid="6e843fe2-ce72-4257-a4c3-1df03728dc95">
   <property name="ireport.zoom" value="1.0"/>
   <property name="ireport.x" value="0"/>
   <property name="ireport.y" value="0"/>
   <field name="COLUMN_0" class="java.lang.String"/>
   <field name="COLUMN_1" class="java.lang.String"/>
   <field name="COLUMN_2" class="java.lang.String"/>
   <field name="COLUMN_3" class="java.lang.String"/>
   <pageHeader>
       <band height="30" splitType="Stretch">
           <staticText>
               <reportElement x="0" y="0" width="69" height="24" uuid="012424cf-712d-4e84-9906-776e1850b85a"/>
               <textElement verticalAlignment="Bottom">
                   <font size="10" isBold="false"/>
               </textElement>
               <text><![CDATA[ID]]></text>
           </staticText>
           <staticText>
               <reportElement x="140" y="0" width="94" height="24" uuid="724d23ca-6ad1-4be5-bae1-77c07dd31ba0"/>
               <textElement textAlignment="Center"/>
               <text><![CDATA[Name]]></text>
           </staticText>
           <staticText>
               <reportElement x="280" y="0" width="69" height="24" uuid="1e85a3f6-ba9d-47a7-8f25-cf37f5b4448d"/>
               <text><![CDATA[Department]]></text>
           </staticText>
           <staticText>
               <reportElement x="420" y="0" width="108" height="24" uuid="044a8958-4960-4fa3-9cd6-c594595c521a"/>
               <text><![CDATA[Email]]></text>
           </staticText>
       </band>
   </pageHeader>
   <detail>
       <band height="30" splitType="Stretch">
           <textField>
               <reportElement x="0" y="0" width="69" height="24" uuid="d844cada-1aa4-4208-9fc1-dcdf62a72235"/>
               <textFieldExpression><![CDATA[$F{COLUMN_0}]]></textFieldExpression>
           </textField>
           <textField>
               <reportElement x="140" y="0" width="94" height="24" uuid="14399970-e399-41e0-b6f9-1218079fd56c"/>
               <textFieldExpression><![CDATA[$F{COLUMN_1}]]></textFieldExpression>
           </textField>
           <textField>
               <reportElement x="280" y="0" width="69" height="24" uuid="b5b0fe03-9b8f-48c6-ba51-c218427028f6"/>
               <textFieldExpression><![CDATA[$F{COLUMN_2}]]></textFieldExpression>
           </textField>
           <textField>
               <reportElement x="420" y="0" width="108" height="24" uuid="c3094477-bb5e-4d5c-a440-8d7c7f2a1d3e"/>
               <textFieldExpression><![CDATA[$F{COLUMN_3}]]></textFieldExpression>
           </textField>
       </band>
   </detail>
</jasperReport>
```
</code>


```
package de.pfeifer_syscon.report;
import java.io.InputStream;
import java.util.HashMap;
import javax.swing.table.DefaultTableModel;
import net.sf.jasperreports.engine.JRException;
import net.sf.jasperreports.engine.JasperCompileManager;
import net.sf.jasperreports.engine.JasperFillManager;
import net.sf.jasperreports.engine.JasperPrint;
import net.sf.jasperreports.engine.JasperReport;
import net.sf.jasperreports.engine.data.JRTableModelDataSource;
import net.sf.jasperreports.view.JasperViewer;
public class SimpleReport {
   DefaultTableModel tableModel;
   public SimpleReport() {
      JasperPrint jasperPrint = null;
      createTableModelData();
      try {
        String name = "report";
        InputStream is = getClass().getResourceAsStream(name + ".jrxml");
        JasperReport report = JasperCompileManager.compileReport(is);
        jasperPrint = JasperFillManager.fillReport(
             report, new HashMap<String, Object>(),
             new JRTableModelDataSource(tableModel));
        JasperViewer jasperViewer = new JasperViewer(jasperPrint);
               jasperViewer.setVisible(true);
        } catch (JRException ex) {
           ex.printStackTrace();
        }
      }
      private void createTableModelData() {
            String[] columnNames = { "Id", "Name", "Department", "Email" };
            String[][] data = {
                { "111", "G Conger", " Orthopaedic", "jim@wheremail.com" },
                { "222", "A Date", "ENT", "adate@somemail.com" },
                { "333", "R Linz", "Paedriatics", "rlinz@heremail.com" },
                { "444", "V Sethi", "Nephrology", "vsethi@whomail.com" },
                { "555", "K Rao", "Orthopaedics", "krao@whatmail.com" },
                { "666", "V Santana", "Nephrology", "vsan@whenmail.com" },
                { "777", "J Pollock", "Nephrology", "jpol@domail.com" },
                { "888", "H David", "Nephrology", "hdavid@donemail.com" },
                { "999", "P Patel", "Nephrology", "ppatel@gomail.com" },
                { "101", "C Comer", "Nephrology", "ccomer@whymail.com" } };
            tableModel = new DefaultTableModel(data, columnNames);
       }
       public static void main(String[] args) {
             new SimpleReport();
       }
}
