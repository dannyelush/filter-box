<template>
  <div>
    <h1>Filter box examples</h1>
    <main>
      <div class="flex">
        <div>
          <h2>Multiple select</h2>
          <filter-box
            v-model="selectedOptions"
            drop-id="first"
            :options="options"
            :search="true"
            :multiple="true"
            :is-expanded="true"
            panel-header="First Options"
            :texts="optionsTexts"
            :sort-selected-first="true"
            :display-max="150"
            />
          <h3>Selected values: {{ selectedOptions }}</h3>
        </div>
        <div>
          <h2>Single select</h2>
          <filter-box
            v-model="selectedOptions2"
            drop-id="second"
            :options="options"
            :search="true"
            :multiple="false"
            :is-expanded="true"
            panel-header="Second Options"
            :texts="optionsTexts"
            :sort-selected-first="false"
            :display-max="100"
            />
          <h3>Selected value: {{ selectedOptions2 }}</h3>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import FilterBox from './components/FilterBox.vue';

const optionsTexts = {
  showMore: 'Show more options',
  searchText: 'Search for options...',
  empty: 'No results',
};

const selectedOptions = ref([]);
const selectedOptions2 = ref(null);

const options = [{
  text: 'Afghanistan',
  value: 'AFG',
  count: '004',
  disabled: true,
},
{
  text: 'Albania',
  value: 'ALB',
  count: '008',
  disabled: true,
},
{
  text: 'Algeria',
  value: 'DZA',
  count: '012',
},
{
  text: 'American Samoa',
  value: 'ASM',
  count: '016',
},
{
  text: 'Andorra',
  value: 'AND',
  count: '020',
},
{
  text: 'Angola',
  value: 'AGO',
  count: '024',
},
{
  text: 'Anguilla',
  value: 'AIA',
  count: '660',
},
{
  text: 'Antarctica',
  value: 'ATA',
  count: '010',
},
{
  text: 'Antigua and Barbuda',
  value: 'ATG',
  count: '028',
},
{
  text: 'Argentina',
  value: 'ARG',
  count: '032',
},
{
  text: 'Armenia',
  value: 'ARM',
  count: '051',
},
{
  text: 'Aruba',
  value: 'ABW',
  count: '533',
},
{
  text: 'Australia',
  value: 'AUS',
  count: '036',
},
{
  text: 'Austria',
  value: 'AUT',
  count: '040',
},
{
  text: 'Azerbaijan',
  value: 'AZE',
  count: '031',
},
{
  text: 'Bahamas',
  value: 'BHS',
  count: '044',
},
{
  text: 'Bahrain',
  value: 'BHR',
  count: '048',
},
{
  text: 'Bangladesh',
  value: 'BGD',
  count: '050',
},
{
  text: 'Barbados',
  value: 'BRB',
  count: '052',
},
{
  text: 'Belarus',
  value: 'BLR',
  count: '112',
},
{
  text: 'Belgium',
  value: 'BEL',
  count: '056',
},
{
  text: 'Belize',
  value: 'BLZ',
  count: '084',
},
{
  text: 'Benin',
  value: 'BEN',
  count: '204',
},
{
  text: 'Bermuda',
  value: 'BMU',
  count: '060',
},
{
  text: 'Bhutan',
  value: 'BTN',
  count: '064',
},
{
  text: 'Bolivia (Plurinational State of)',
  value: 'BOL',
  count: '068',
},
{
  text: 'Bonaire, Sint Eustatius and Saba',
  value: 'BES',
  count: '535',
},
{
  text: 'Bosnia and Herzegovina',
  value: 'BIH',
  count: '070',
},
{
  text: 'Botswana',
  value: 'BWA',
  count: '072',
},
{
  text: 'Bouvet Island',
  value: 'BVT',
  count: '074',
},
{
  text: 'Brazil',
  value: 'BRA',
  count: '076',
},
{
  text: 'British Indian Ocean Territory',
  value: 'IOT',
  count: '086',
},
{
  text: 'United States Minor Outlying Islands',
  value: 'UMI',
  count: '581',
},
{
  text: 'Virgin Islands (British)',
  value: 'VGB',
  count: '092',
},
{
  text: 'Virgin Islands (U.S.)',
  value: 'VIR',
  count: '850',
},
{
  text: 'Brunei Darussalam',
  value: 'BRN',
  count: '096',
},
{
  text: 'Bulgaria',
  value: 'BGR',
  count: '100',
},
{
  text: 'Burkina Faso',
  value: 'BFA',
  count: '854',
},
{
  text: 'Burundi',
  value: 'BDI',
  count: '108',
},
{
  text: 'Cambodia',
  value: 'KHM',
  count: '116',
},
{
  text: 'Cameroon',
  value: 'CMR',
  count: '120',
},
{
  text: 'Canada',
  value: 'CAN',
  count: '124',
},
{
  text: 'Cabo Verde',
  value: 'CPV',
  count: '132',
},
{
  text: 'Cayman Islands',
  value: 'CYM',
  count: '136',
},
{
  text: 'Central African Republic',
  value: 'CAF',
  count: '140',
},
{
  text: 'Chad',
  value: 'TCD',
  count: '148',
},
{
  text: 'Chile',
  value: 'CHL',
  count: '152',
},
{
  text: 'China',
  value: 'CHN',
  count: '156',
},
{
  text: 'Christmas Island',
  value: 'CXR',
  count: '162',
},
{
  text: 'Cocos (Keeling) Islands',
  value: 'CCK',
  count: '166',
},
{
  text: 'Colombia',
  value: 'COL',
  count: '170',
},
{
  text: 'Comoros',
  value: 'COM',
  count: '174',
},
{
  text: 'Congo',
  value: 'COG',
  count: '178',
},
{
  text: 'Congo (Democratic Republic of the)',
  value: 'COD',
  count: '180',
},
{
  text: 'Cook Islands',
  value: 'COK',
  count: '184',
},
{
  text: 'Costa Rica',
  value: 'CRI',
  count: '188',
},
{
  text: 'Croatia',
  value: 'HRV',
  count: '191',
},
{
  text: 'Cuba',
  value: 'CUB',
  count: '192',
},
{
  text: 'Curaçao',
  value: 'CUW',
  count: '531',
},
{
  text: 'Cyprus',
  value: 'CYP',
  count: '196',
},
{
  text: 'Czech Republic',
  value: 'CZE',
  count: '203',
},
{
  text: 'Denmark',
  value: 'DNK',
  count: '208',
},
{
  text: 'Djibouti',
  value: 'DJI',
  count: '262',
},
{
  text: 'Dominica',
  value: 'DMA',
  count: '212',
},
{
  text: 'Dominican Republic',
  value: 'DOM',
  count: '214',
},
{
  text: 'Ecuador',
  value: 'ECU',
  count: '218',
},
{
  text: 'Egypt',
  value: 'EGY',
  count: '818',
},
{
  text: 'El Salvador',
  value: 'SLV',
  count: '222',
},
{
  text: 'Equatorial Guinea',
  value: 'GNQ',
  count: '226',
},
{
  text: 'Eritrea',
  value: 'ERI',
  count: '232',
},
{
  text: 'Estonia',
  value: 'EST',
  count: '233',
},
{
  text: 'Ethiopia',
  value: 'ETH',
  count: '231',
},
{
  text: 'Falkland Islands (Malvinas)',
  value: 'FLK',
  count: '238',
},
{
  text: 'Faroe Islands',
  value: 'FRO',
  count: '234',
},
{
  text: 'Fiji',
  value: 'FJI',
  count: '242',
},
{
  text: 'Finland',
  value: 'FIN',
  count: '246',
},
{
  text: 'France',
  value: 'FRA',
  count: '250',
},
{
  text: 'French Guiana',
  value: 'GUF',
  count: '254',
},
{
  text: 'French Polynesia',
  value: 'PYF',
  count: '258',
},
{
  text: 'French Southern Territories',
  value: 'ATF',
  count: '260',
},
{
  text: 'Gabon',
  value: 'GAB',
  count: '266',
},
{
  text: 'Gambia',
  value: 'GMB',
  count: '270',
},
{
  text: 'Georgia',
  value: 'GEO',
  count: '268',
},
{
  text: 'Germany',
  value: 'DEU',
  count: '276',
},
{
  text: 'Ghana',
  value: 'GHA',
  count: '288',
},
{
  text: 'Gibraltar',
  value: 'GIB',
  count: '292',
},
{
  text: 'Greece',
  value: 'GRC',
  count: '300',
},
{
  text: 'Greenland',
  value: 'GRL',
  count: '304',
},
{
  text: 'Grenada',
  value: 'GRD',
  count: '308',
},
{
  text: 'Guadeloupe',
  value: 'GLP',
  count: '312',
},
{
  text: 'Guam',
  value: 'GUM',
  count: '316',
},
{
  text: 'Guatemala',
  value: 'GTM',
  count: '320',
},
{
  text: 'Guernsey',
  value: 'GGY',
  count: '831',
},
{
  text: 'Guinea',
  value: 'GIN',
  count: '324',
},
{
  text: 'Guinea-Bissau',
  value: 'GNB',
  count: '624',
},
{
  text: 'Guyana',
  value: 'GUY',
  count: '328',
},
{
  text: 'Haiti',
  value: 'HTI',
  count: '332',
},
{
  text: 'Heard Island and McDonald Islands',
  value: 'HMD',
  count: '334',
},
{
  text: 'Holy See',
  value: 'VAT',
  count: '336',
},
{
  text: 'Honduras',
  value: 'HND',
  count: '340',
},
{
  text: 'Hong Kong',
  value: 'HKG',
  count: '344',
},
{
  text: 'Hungary',
  value: 'HUN',
  count: '348',
},
{
  text: 'Iceland',
  value: 'ISL',
  count: '352',
},
{
  text: 'India',
  value: 'IND',
  count: '356',
},
{
  text: 'Indonesia',
  value: 'IDN',
  count: '360',
},
{
  text: "Côte d'Ivoire",
  value: 'CIV',
  count: '384',
},
{
  text: 'Iran (Islamic Republic of)',
  value: 'IRN',
  count: '364',
},
{
  text: 'Iraq',
  value: 'IRQ',
  count: '368',
},
{
  text: 'Ireland',
  value: 'IRL',
  count: '372',
},
{
  text: 'Isle of Man',
  value: 'IMN',
  count: '833',
},
{
  text: 'Israel',
  value: 'ISR',
  count: '376',
},
{
  text: 'Italy',
  value: 'ITA',
  count: '380',
},
{
  text: 'Jamaica',
  value: 'JAM',
  count: '388',
},
{
  text: 'Japan',
  value: 'JPN',
  count: '392',
},
{
  text: 'Jersey',
  value: 'JEY',
  count: '832',
},
{
  text: 'Jordan',
  value: 'JOR',
  count: '400',
},
{
  text: 'Kazakhstan',
  value: 'KAZ',
  count: '398',
},
{
  text: 'Kenya',
  value: 'KEN',
  count: '404',
},
{
  text: 'Kiribati',
  value: 'KIR',
  count: '296',
},
{
  text: 'Kuwait',
  value: 'KWT',
  count: '414',
},
{
  text: 'Kyrgyzstan',
  value: 'KGZ',
  count: '417',
},
{
  text: "Lao People's Democratic Republic",
  value: 'LAO',
  count: '418',
},
{
  text: 'Latvia',
  value: 'LVA',
  count: '428',
},
{
  text: 'Lebanon',
  value: 'LBN',
  count: '422',
},
{
  text: 'Lesotho',
  value: 'LSO',
  count: '426',
},
{
  text: 'Liberia',
  value: 'LBR',
  count: '430',
},
{
  text: 'Libya',
  value: 'LBY',
  count: '434',
},
{
  text: 'Liechtenstein',
  value: 'LIE',
  count: '438',
},
{
  text: 'Lithuania',
  value: 'LTU',
  count: '440',
},
{
  text: 'Luxembourg',
  value: 'LUX',
  count: '442',
},
{
  text: 'Macao',
  value: 'MAC',
  count: '446',
},
{
  text: 'Macedonia (the former Yugoslav Republic of)',
  value: 'MKD',
  count: '807',
},
{
  text: 'Madagascar',
  value: 'MDG',
  count: '450',
},
{
  text: 'Malawi',
  value: 'MWI',
  count: '454',
},
{
  text: 'Malaysia',
  value: 'MYS',
  count: '458',
},
{
  text: 'Maldives',
  value: 'MDV',
  count: '462',
},
{
  text: 'Mali',
  value: 'MLI',
  count: '466',
},
{
  text: 'Malta',
  value: 'MLT',
  count: '470',
},
{
  text: 'Marshall Islands',
  value: 'MHL',
  count: '584',
},
{
  text: 'Martinique',
  value: 'MTQ',
  count: '474',
},
{
  text: 'Mauritania',
  value: 'MRT',
  count: '478',
},
{
  text: 'Mauritius',
  value: 'MUS',
  count: '480',
},
{
  text: 'Mayotte',
  value: 'MYT',
  count: '175',
},
{
  text: 'Mexico',
  value: 'MEX',
  count: '484',
},
{
  text: 'Micronesia (Federated States of)',
  value: 'FSM',
  count: '583',
},
{
  text: 'Moldova (Republic of)',
  value: 'MDA',
  count: '498',
},
{
  text: 'Monaco',
  value: 'MCO',
  count: '492',
},
{
  text: 'Mongolia',
  value: 'MNG',
  count: '496',
},
{
  text: 'Montenegro',
  value: 'MNE',
  count: '499',
},
{
  text: 'Montserrat',
  value: 'MSR',
  count: '500',
},
{
  text: 'Morocco',
  value: 'MAR',
  count: '504',
},
{
  text: 'Mozambique',
  value: 'MOZ',
  count: '508',
},
{
  text: 'Myanmar',
  value: 'MMR',
  count: '104',
},
{
  text: 'Namibia',
  value: 'NAM',
  count: '516',
},
{
  text: 'Nauru',
  value: 'NRU',
  count: '520',
},
{
  text: 'Nepal',
  value: 'NPL',
  count: '524',
},
{
  text: 'Netherlands',
  value: 'NLD',
  count: '528',
},
{
  text: 'New Caledonia',
  value: 'NCL',
  count: '540',
},
{
  text: 'New Zealand',
  value: 'NZL',
  count: '554',
},
{
  text: 'Nicaragua',
  value: 'NIC',
  count: '558',
},
{
  text: 'Niger',
  value: 'NER',
  count: '562',
},
{
  text: 'Nigeria',
  value: 'NGA',
  count: '566',
},
{
  text: 'Niue',
  value: 'NIU',
  count: '570',
},
{
  text: 'Norfolk Island',
  value: 'NFK',
  count: '574',
},
{
  text: "Korea (Democratic People's Republic of)",
  value: 'PRK',
  count: '408',
},
{
  text: 'Northern Mariana Islands',
  value: 'MNP',
  count: '580',
},
{
  text: 'Norway',
  value: 'NOR',
  count: '578',
},
{
  text: 'Oman',
  value: 'OMN',
  count: '512',
},
{
  text: 'Pakistan',
  value: 'PAK',
  count: '586',
},
{
  text: 'Palau',
  value: 'PLW',
  count: '585',
},
{
  text: 'Palestine, State of',
  value: 'PSE',
  count: '275',
},
{
  text: 'Panama',
  value: 'PAN',
  count: '591',
},
{
  text: 'Papua New Guinea',
  value: 'PNG',
  count: '598',
},
{
  text: 'Paraguay',
  value: 'PRY',
  count: '600',
},
{
  text: 'Peru',
  value: 'PER',
  count: '604',
},
{
  text: 'Philippines',
  value: 'PHL',
  count: '608',
},
{
  text: 'Pitcairn',
  value: 'PCN',
  count: '612',
},
{
  text: 'Poland',
  value: 'POL',
  count: '616',
},
{
  text: 'Portugal',
  value: 'PRT',
  count: '620',
},
{
  text: 'Puerto Rico',
  value: 'PRI',
  count: '630',
},
{
  text: 'Qatar',
  value: 'QAT',
  count: '634',
},
{
  text: 'Republic of Kosovo',
  value: 'KOS',
  count: null,
},
{
  text: 'Réunion',
  value: 'REU',
  count: '638',
},
{
  text: 'Romania',
  value: 'ROU',
  count: '642',
},
{
  text: 'Russian Federation',
  value: 'RUS',
  count: '643',
},
{
  text: 'Rwanda',
  value: 'RWA',
  count: '646',
},
{
  text: 'Saint Barthélemy',
  value: 'BLM',
  count: '652',
},
{
  text: 'Saint Helena, Ascension and Tristan da Cunha',
  value: 'SHN',
  count: '654',
},
{
  text: 'Saint Kitts and Nevis',
  value: 'KNA',
  count: '659',
},
{
  text: 'Saint Lucia',
  value: 'LCA',
  count: '662',
},
{
  text: 'Saint Martin (French part)',
  value: 'MAF',
  count: '663',
},
{
  text: 'Saint Pierre and Miquelon',
  value: 'SPM',
  count: '666',
},
{
  text: 'Saint Vincent and the Grenadines',
  value: 'VCT',
  count: '670',
},
{
  text: 'Samoa',
  value: 'WSM',
  count: '882',
},
{
  text: 'San Marino',
  value: 'SMR',
  count: '674',
},
{
  text: 'Sao Tome and Principe',
  value: 'STP',
  count: '678',
},
{
  text: 'Saudi Arabia',
  value: 'SAU',
  count: '682',
},
{
  text: 'Senegal',
  value: 'SEN',
  count: '686',
},
{
  text: 'Serbia',
  value: 'SRB',
  count: '688',
},
{
  text: 'Seychelles',
  value: 'SYC',
  count: '690',
},
{
  text: 'Sierra Leone',
  value: 'SLE',
  count: '694',
},
{
  text: 'Singapore',
  value: 'SGP',
  count: '702',
},
{
  text: 'Sint Maarten (Dutch part)',
  value: 'SXM',
  count: '534',
},
{
  text: 'Slovakia',
  value: 'SVK',
  count: '703',
},
{
  text: 'Slovenia',
  value: 'SVN',
  count: '705',
},
{
  text: 'Solomon Islands',
  value: 'SLB',
  count: '090',
},
{
  text: 'Somalia',
  value: 'SOM',
  count: '706',
},
{
  text: 'South Africa',
  value: 'ZAF',
  count: '710',
},
{
  text: 'South Georgia and the South Sandwich Islands',
  value: 'SGS',
  count: '239',
},
{
  text: 'Korea (Republic of)',
  value: 'KOR',
  count: '410',
},
{
  text: 'South Sudan',
  value: 'SSD',
  count: '728',
},
{
  text: 'Spain',
  value: 'ESP',
  count: '724',
},
{
  text: 'Sri Lanka',
  value: 'LKA',
  count: '144',
},
{
  text: 'Sudan',
  value: 'SDN',
  count: '729',
},
{
  text: 'Suriname',
  value: 'SUR',
  count: '740',
},
{
  text: 'Svalbard and Jan Mayen',
  value: 'SJM',
  count: '744',
},
{
  text: 'Swaziland',
  value: 'SWZ',
  count: '748',
},
{
  text: 'Sweden',
  value: 'SWE',
  count: '752',
},
{
  text: 'Switzerland',
  value: 'CHE',
  count: '756',
},
{
  text: 'Syrian Arab Republic',
  value: 'SYR',
  count: '760',
},
{
  text: 'Taiwan',
  value: 'TWN',
  count: '158',
},
{
  text: 'Tajikistan',
  value: 'TJK',
  count: '762',
},
{
  text: 'Tanzania, United Republic of',
  value: 'TZA',
  count: '834',
},
{
  text: 'Thailand',
  value: 'THA',
  count: '764',
},
{
  text: 'Timor-Leste',
  value: 'TLS',
  count: '626',
},
{
  text: 'Togo',
  value: 'TGO',
  count: '768',
},
{
  text: 'Tokelau',
  value: 'TKL',
  count: '772',
},
{
  text: 'Tonga',
  value: 'TON',
  count: '776',
},
{
  text: 'Trinidad and Tobago',
  value: 'TTO',
  count: '780',
},
{
  text: 'Tunisia',
  value: 'TUN',
  count: '788',
},
{
  text: 'Turkey',
  value: 'TUR',
  count: '792',
},
{
  text: 'Turkmenistan',
  value: 'TKM',
  count: '795',
},
{
  text: 'Turks and Caicos Islands',
  value: 'TCA',
  count: '796',
},
{
  text: 'Tuvalu',
  value: 'TUV',
  count: '798',
},
{
  text: 'Uganda',
  value: 'UGA',
  count: '800',
},
{
  text: 'Ukraine',
  value: 'UKR',
  count: '804',
},
{
  text: 'United Arab Emirates',
  value: 'ARE',
  count: '784',
},
{
  text: 'United Kingdom of Great Britain and Northern Ireland',
  value: 'GBR',
  count: '826',
},
{
  text: 'United States of America',
  value: 'USA',
  count: '840',
},
{
  text: 'Uruguay',
  value: 'URY',
  count: '858',
},
{
  text: 'Uzbekistan',
  value: 'UZB',
  count: '860',
},
{
  text: 'Vanuatu',
  value: 'VUT',
  count: '548',
},
{
  text: 'Venezuela (Bolivarian Republic of)',
  value: 'VEN',
  count: '862',
},
{
  text: 'Viet Nam',
  value: 'VNM',
  count: '704',
},
{
  text: 'Wallis and Futuna',
  value: 'WLF',
  count: '876',
},
{
  text: 'Western Sahara',
  value: 'ESH',
  count: '732',
},
{
  text: 'Yemen',
  value: 'YEM',
  count: '887',
},
{
  text: 'Zambia',
  value: 'ZMB',
  count: '894',
},
{
  text: 'Zimbabwe',
  value: 'ZWE',
  count: '716',
}];
</script>

<style scoped>
h1 {
  text-align: center;
  color: #ffffff;
  text-shadow: 1px 1px #110316;
}
main {
  margin: 30px auto 0;
  background: #ffffff;
  align-items: center;
  justify-items: center;
  max-width: 1000px;
  padding: 20px;
  border-radius: 5px;
}

.flex {
  display: flex;
  justify-content: space-between;
  flex-wrap: wrap;
}

main .flex > div {
  width: 400px;
}
</style>
