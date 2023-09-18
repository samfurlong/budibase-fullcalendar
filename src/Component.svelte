<script>
  import { getContext } from 'svelte';
  import '@fullcalendar/core/locales-all';
  import FullCalendar from 'svelte-fullcalendar';
  import daygridPlugin from '@fullcalendar/daygrid';
  import timeGridPlugin from '@fullcalendar/timegrid';
  import listPlugin from '@fullcalendar/list';
  import interactionPlugin from '@fullcalendar/interaction';
  import { onMount } from 'svelte';
  import { langs, codeLang } from './lang';

  export let language;
  export let calendarClick;
  export let calendarDrop;

  export let tag1;
  export let mappingId;
  export let mappingTitle;
  export let mappingDate;
  export let mappingStart;
  export let mappingEnd;
  export let editable;

  export let tag2;
  export let mappingId2;
  export let mappingTitle2;
  export let mappingDate2;
  export let mappingStart2;
  export let mappingEnd2;
  export let editable2;

  export let tag3;
  export let mappingId3;
  export let mappingTitle3;
  export let mappingDate3;
  export let mappingStart3;
  export let mappingEnd3;
  export let editable3;

  export let dataProvider;
  export let dataProvider2;
  export let dataProvider3;

  export let mappingColor;
  export let mappingColor2;
  export let mappingColor3;

  export let allday;
  export let allday2;
  export let allday3;

  export let headerOptionsStart;
  export let headerOptionsCenter;
  export let headerOptionsEnd;

  let eventsList = [];
  onMount(() => {
    if (eventsList.length > 0) {
      eventsList = [];
    }
    if (dataProvider.rows) {
      dataProvider.rows.forEach((event) => {
        let eventColor = mappingColor ?? '#313131';
        eventsList.push({
          id: parseInt(event[mappingId]),
          title: event[mappingTitle],
          date: event[mappingDate],
          start: event[mappingStart],
          end: event[mappingEnd],
          color: eventColor,
          event: event,
          allDay: allday,
          editable: editable,
          tag: tag1,
        });
      });
    }
    if (dataProvider2.rows) {
      dataProvider2.rows.forEach((event) => {
        let eventColor2 = mappingColor2 ?? '#eb4034';
        eventsList.push({
          title: event[mappingTitle2],
          date: event[mappingDate2],
          start: event[mappingStart2],
          end: event[mappingEnd2],
          color: eventColor2,
          event: event,
          allDay: allday2,
          editable: editable2,
          tag: tag2,
        });
      });
    }
    if (dataProvider3.rows) {
      dataProvider3.rows.forEach((event) => {
        let eventColor3 = mappingColor3 ?? '#eb4034';
        eventsList.push({
          title: event[mappingTitle3],
          date: event[mappingDate3],
          start: event[mappingStart3],
          end: event[mappingEnd3],
          color: eventColor3,
          event: event,
          allDay: allday3,
          editable: editable3,
          tag: tag3,
        });
      });
    }
    eventsList = eventsList;
  });

  let options = {
    headerToolbar: {
      start: headerOptionsStart,
      center: headerOptionsCenter,
      end: headerOptionsEnd,
    },
    plugins: [daygridPlugin, listPlugin, timeGridPlugin, interactionPlugin],
    initialDate: Date.now(),
    locale: language,
    dayMaxEvents: true,
    eventClick: (event) => {
      calendarClick({
        value: event.event,
      });
    },
    eventDrop: (event) => {
      calendarDrop({
        value: event.event,
      });
    },
    events: eventsList,
    eventColor: '#378006',
    theme: true,
    ...langs[codeLang(language)],
  };
  const { styleable } = getContext('sdk');
  const component = getContext('component');
</script>

<div use:styleable={$component.styles}>
  <FullCalendar {options} />
</div>
